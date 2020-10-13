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




