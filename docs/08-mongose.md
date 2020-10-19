modeling schema with mongose  

installing mongose  


```sh  
npm install --save mongoose @nestjs/mongoose
install --save-dev @types/mongoose i
```

example mongodb docker container  

```txt
version: '3'
volumes:
 mongo_data:
services:
 mongo:
 image: mongo:latest
 ports:
 - "27017:27017"
 volumes:
 - mongo_data:/data/db
```


example mongoose module  

```ts
// AppModule
import { MongooseModule } from '@nestjs/mongoose';
@Module({
	imports: [
		MongooseModule.forRoot('mongodb://mongo:27017/nest'),
	],
})
export class AppModule {}

// entry.schema.ts
import { Schema } from 'mongoose';
export const EntrySchema = new mongoose.Schema({
 _id: Schema.Types.ObjectId,
 title: String,
 body: String,
 image: String,
 created_at: Date,
});

// entry.module.ts
import { Module } from '@nestjs/common';
import { MongooseModule } from '@nestjs/mongoose';
import { EntrySchema } from './entry.schema';
@Module({
	imports: [
		MongooseModule.forFeature([{ name: 'Entry', schema: EntrySchema }]),
	],
})
export class EntriesModule {}

// entry.interface.ts
import { Document } from 'mongoose';
export interface Entry extends Document {
	readonly _id: string;
	readonly title: string;
	readonly body: string;
	readonly image: string;
	readonly created_at: Date;
}
```


example services   

```ts
// entries.service.ts

import { Component } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model, Types } from 'mongoose';
import { EntrySchema } from './entry.schema';
import { Entry } from './entry.interface';
@Component()
export class EntriesService {
	constructor(
		@InjectModel(EntrySchema) private readonly entryModel: Model<Entry>
	) {}
	// this method retrieves all entries
	findAll() {
		return this.entryModel.find().exec();
	}
	// this method retrieves only one entry, by entry ID
	findById(id: string) {
		return this.entryModel.findById(id).exec();
	}
	// this method saves an entry in the database
	create(entry) {
		entry._id = new Types.ObjectId();
		const createdEntry = new this.entryModel(entry);
		return createdEntry.save();
	}
}

// entries.controller.ts
import { Controller, Get, Post, Body, Param } from '@nestjs/common';
import { EntriesService } from './entry.service';
@Controller('entries')
export class EntriesController {
	constructor(private readonly entriesSrv: EntriesService) {}
	@Get()
	findAll() {
		return this.entriesSrv.findAll();
	}
	@Get(':entryId')
	findById(@Param('entryId') entryId) {
		return this.entriesSrv.findById(entryId);
	}
	@Post()
	create(@Body() entry) {
		return this.entriesSrv.create(entry);
	}
}
```
