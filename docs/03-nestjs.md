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


provider   
