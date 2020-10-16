Basic setup   

```sh
npm i -g @nestjs/cli
cd ../dist/
nest new 03-nest/
```

or use typescript git.  

```sh
git clone https://github.com/nestjs/typescript-starter.git project
cd project
npm install
npm run start
```

controller example.   

```ts  

import {Controller, Get, Post, Res, HttpStatus} from "@nestjs"
import {Response} from 'express';

@Controller('cats')
export class CatsController {
	@Post()
	create(@Res() res: Response){
		res.status(HttpStatus.OK).json([]);
	}
	@Get()
	findAll(@Req request: Request, @Res() res: Response){
		res.status(HttpStatus.OK).json([]);
	}
	@Put(':id')
	update(@Param('id') id: string, @Body() updateCatDto: UpdateCatDto){
		return `This actions updates a#${id} cat`;
	}
	@Delete(':id')
	remove(@Param('id') id: string){
		return `this actions removes #${id} cat`;
	}
}


```
 controller decorator  
 
```ts
// request, response 
@Request(), @Req()	req
@Response(), @Res()*	res
@Next()	next
@Session()	req.session
@Param(key?: string)	req.params / req.params[key]
@Body(key?: string)	req.body / req.body[key]
@Query(key?: string)	req.query / req.query[key]
@Headers(name?: string)	req.headers / req.headers[name]
@Ip()	req.ip
@HostParam()	req.hosts

// http method
@Get, @Post, @Put(), @Delete(), @Patch(), @Options(), and @Head(),All()

// status code
@HttpCode(204)

// headers
@Header('Cache-Control', 'none');

// redirect
@Redirect('/foo.bar', 301);

// subdomain routing
@Controller({ host: 'admin.example.com' })
export class AdminController {
  @Get()
  index(): string {
    return 'Admin page';
  }
}

// async

@Get()
async findAll(): Promise<any[]> {
  return [];
}
```


Request payload with Data transfer object schema  

```ts 
// file cat.dto.ts
export class CreateCatDto {
  name: string;
  age: number;
  breed: string;
}
// cats.controller.ts

@Post()
async create(@Body() createCatDto: CreateCatDto) {
	return 'This action adds a new cat';
}

```

Using the controller at module   

```ts
//app.module.ts
import {Module} from '@nestjs/common';
import {CatsController} from './cats/cats.controller';

@Module({
	controllers: [CatsController],
});

export class AppModule {}
```


Providers   

```ts
// cats.services.ts
import { Injectable } from '@nestjs/common';
import { Cat } from './interfaces/cat.interface';

@Injectable()
export class CatsService {
	private readonly cats: Cat[] = [];

	create(cat: Cat) {
		this.cats.push(cat);
	}

	findAll(): Cat[] {
		return this.cats;
	}
}

// cat.interfaces.ts
export interface Cat {
	name: string;
	age: number;
	breed: string;
}

// cats.controller.ts
import { Controller, Get, Post, Body } from '@nestjs/common';
import { CreateCatDto } from './dto/create-cat.dto';
import { CatsService } from './cats.service';
import { Cat } from './interfaces/cat.interface';

@Controller('cats')
export class CatsController {
	constructor(private catsService: CatsService) {}

	@Post()
	async create(@Body() createCatDto: CreateCatDto) {
		this.catsService.create(createCatDto);
	}

	@Get()
	async findAll(): Promise<Cat[]> {
		return this.catsService.findAll();
	}
}

// app.module.ts
// registering
import { Module } from '@nestjs/common';
import { CatsController } from './cats/cats.controller';
import { CatsService } from './cats/cats.service';

@Module({
	controllers: [CatsController],
	providers: [CatsService],
})
export class AppModule {}
```

common directory structure   

```txt
src
  cats
    dto
      create-cat.dto.ts
    interfaces
      cat.interface.ts
    cats.service.ts
    cats.controller.ts
    app.module.ts
    main.ts
```

registering module at global scopes

```ts
{
	global: true,
	module: DatabaseModule,
	providers: providers,
	exports: providers,
}

```
Middleware  
task:   
- change the req, res object, 
- end or terminate req, res cycle, 


middleware should in injectable decorator.   

example of usage.  

```ts
// logger.middleware.ts
import {Injectable, NestMiddleware} from '@nestjs/common';
import {Request, Response} from 'express';

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
	use(req: Request, res: Response, next: Function){
		console.log('Request...');
		next();
	}
}

// applying it
// app.modules.ts

import { Module, NestModule, MiddlewareConsumer } from '@nestjs/common';
import { LoggerMiddleware } from './common/middleware/logger.middleware';
import { CatsModule } from './cats/cats.module';

@Module({
	imports: [CatsModule],
})
export class AppModule implements NestModule {
	configure(consumer: MiddlewareConsumer) {
		consumer
			.apply(LoggerMiddleware)
			.forRoutes('cats'); // only apply for routes cats
		//.forRoutes({ path: 'cats', method: RequestMethod.GET });

	}
}
```

extending routes through middleware  

```ts
consumer
	.apply(LoggerMiddleware)
	.exclude(
		{ path: 'cats', method: RequestMethod.GET },
		{ path: 'cats', method: RequestMethod.POST },
		'cats/(.*)',
	)
	.forRoutes(CatsController);
```

functional middleware  

```ts
// logger.middleware.ts

import { Request, Response } from 'express';

export function logger(req: Request, res: Response, next: Function) {
	console.log(`Request...`);
	next();
};

// app.module.ts

consumer
	.apply(logger)
	.forRoutes(CatsController);
```

Multiple middleware  
```ts
consumer.apply(cors(), helmet(), logger).forRoutes(CatsController);
```

Exception filters

```ts
// cats.controller.ts

@Get
async findAll(){
	throw new HttpException('Forbidden', HttpStatus.FORBIDDEN);	
}

//it would return the result
{
	"statusCode": 403,
	"message": "Forbidden"
}
```


Default Exception Filter  

```json
{
	"statusCode": 500,
	"message": "Internal server error"
}

```

Override default exceptions  

```ts

@Get()
async findAll() {
	throw new HttpException({
		status: HttpStatus.FORBIDDEN,
		error: 'This is a custom message',
	}, HttpStatus.FORBIDDEN);
}

//result
{
	"status" : 403,
	"error": 'This is a custom message'
}
```

Custom Exception

```ts
//forbidden.exception.ts

export class ForbiddenException extends HttpException {
	constructor() {
		super('Forbidden', HttpStatus.FORBIDDEN);
	}
}

// cats.controller.ts

@Get()
async findAll() {
	throw new ForbiddenException();
}

// builtins
// BadRequestException
// UnauthorizedException
// NotFoundException
// ForbiddenException
// NotAcceptableException
// RequestTimeoutException
// ConflictException
// GoneException
// HttpVersionNotSupportedException
// PayloadTooLargeException
// UnsupportedMediaTypeException
// UnprocessableEntityException
// InternalServerErrorException
// NotImplementedException
// ImATeapotException
// MethodNotAllowedException
// BadGatewayException
// ServiceUnavailableException
// GatewayTimeoutException
// PreconditionFailedException

```

Exceptions Filter

```ts
//http-exception.filter.ts

import { ExceptionFilter, Catch, ArgumentsHost, HttpException } from '@nestjs/common';
import { Request, Response } from 'express';

@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const request = ctx.getRequest<Request>();
    const status = exception.getStatus();

    response
      .status(status)
      .json({
        statusCode: status,
        timestamp: new Date().toISOString(),
        path: request.url,
      });
  }
}
```

Pipes, Transformation or Validation  

builtin pipe  

```ts
ValidationPipe
ParseIntPipe
ParseBoolPipe
ParseArrayPipe
ParseUUIDPipe
DefaultValuePipe
```

```ts
@Get(':id')
async findOne(@Param('id', ParseIntPipe) id: number) {
	return this.catsService.findOne(id);
}

// GET localhost:3000/abc

{
	"statusCode": 400,
	"message": "Validation failed (numeric string is expected)",
	"error": "Bad Request"
}
```

Guards  

exceuted in order. middlewares -> guards -> interceptor -> pipe

example authorization guard.

```ts
//auth.guard.ts
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { Observable } from 'rxjs';

@Injectable()
export class AuthGuard implements CanActivate {
	canActivate(
		context: ExecutionContext,
	): boolean | Promise<boolean> | Observable<boolean> {
		const request = context.switchToHttp().getRequest();
		return validateRequest(request);
	}
}

// if it true, request will be processed, if it false, nest will deny the request

// roles.guards.ts
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { Observable } from 'rxjs';

@Injectable()
export class RolesGuard implements CanActivate {
	canActivate(
		context: ExecutionContext,
	): boolean | Promise<boolean> | Observable<boolean> {
		return true;
	}
}

// binding guards in controller

@Controller('cats')
@UseGuards(RolesGuard)
export class CatsController {}

const app = await NestFactory.create(AppModule);
app.useGlobalGuards(new RolesGuard());


import { Module } from '@nestjs/common';
import { APP_GUARD } from '@nestjs/core';


@Module({
	providers: [
		{
			provide: APP_GUARD,
			useClass: RolesGuard,
		},
	],
})
export class AppModule {}


// roles per handler
// cats.controller.ts

@Post()
@SetMetadata('roles', ['admin'])
async create(@Body() createCatDto: CreateCatDto) {
	this.catsService.create(createCatDto);
}

// roles.decorator.ts

import { SetMetadata } from '@nestjs/common';

export const Roles = (...roles: string[]) => SetMetadata('roles', roles);

// cats.controller.ts

@Post()
@Roles('admin')
async create(@Body() createCatDto: CreateCatDto) {
	this.catsService.create(createCatDto);
}

```


example using roles guards

```ts

import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { Reflector } from '@nestjs/core';

@Injectable()
export class RolesGuard implements CanActivate {
	constructor(private reflector: Reflector) {}

	canActivate(context: ExecutionContext): boolean {
		const roles = this.reflector.get<string[]>('roles', context.getHandler());
		if (!roles) {
			return true;
		}
		const request = context.switchToHttp().getRequest();
		const user = request.user;
		return matchRoles(roles, user.roles);
	}
}
```


Interceptors   

example loggin interceptors

```ts  
// logging.interceptors.ts
import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';

@Injectable()
export class LoggingInterceptor implements NestInterceptor {
	intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
		console.log('Before...');

		const now = Date.now();
		return next
			.handle()
			.pipe(
				tap(() => console.log(`After... ${Date.now() - now}ms`)),
			);
	}
}

// in use
// cats.controller.ts

@UseInterceptors(LoggingInterceptor)
export class CatsController {}

// in a module

import { Module } from '@nestjs/common';
import { APP_INTERCEPTOR } from '@nestjs/core';


@Module({
	providers: [
		{
			provide: APP_INTERCEPTOR,
			useClass: LoggingInterceptor,
		},
	],
})
export class AppModule {}
```

example transform interceptors  

```ts
// transform.interceptor.ts

import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';

export interface Response<T> {
	data: T;
}


@Injectable()
export class TransformInterceptor<T> implements NestInterceptor<T, Response<T>> {
	intercept(context: ExecutionContext, next: CallHandler): Observable<Response<T>> {
		return next.handle().pipe(map(data => ({ data })));
	}
}

```
