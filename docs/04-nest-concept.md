

Testing 
testing using jest, and supertetst.  

```sh
npm i --save-dev @nestjs/testing
```

example usage of unit testing  
```ts
// cats.controller.spec.ts

import { CatsController } from './cats.controller';
import { CatsServices } from './cats.services';

describe('CatsController', () =>{
	let catsController: CatsController;
	let catsServices: CatsService;

	beforeEach(()=>{
		catsService = new CatsService();
		catsController = new CatsController(catsService);
		
	});

	describe('findAll', () =>{
		it('should return an array of cats', async () =>{
			const result = ['test'];
			jest.spyOn(catsService, 'findAll').mockImplementation(() => result);

			expect(await catsController.findAll()).toBe(result);
			
		});
		
	});		
});

```
Testing utility  

```ts
// cats.controller.spec.ts
import { Test } from '@nestjs/testing';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

describe('CatsController', ()=>{
	let catsController: CatsController;
	let catsService: CatsService;

	beforeEach(async ()=>{
		const moduleRef = await Test.createTestingModule({
			controllers: [CatsController],
			providers  : [CatsService]	
		}).compile();

		catsService = moduleRef.get<CatsService>(CatsService);
		catsController = moduleRef.get<CatsController>(CatsController);
		
	});
});

```
