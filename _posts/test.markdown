```objc
NSManagedObjectContext *privateMOC = [[NSManagedObjectContext alloc] initWithConcurrencyType:NSPrivateQueueConcurrencyType];
            privateMOC.persistentStoreCoordinator = [CoreDataStack main].coordinator;
            privateMOC.mergePolicy = NSMergeByPropertyObjectTrumpMergePolicy;
            
            [privateMOC performBlock:^{
                NSMutableArray *messages = [NSMutableArray arrayWithCapacity:data.count];
                NSEntityDescription *ent = [NSEntityDescription entityForName:@"Message" inManagedObjectContext:privateMOC];
                NSFetchRequest *fetchRequest = [[NSFetchRequest alloc] init];
                fetchRequest.entity = ent;
                for (NSDictionary *dic in data) {
                    Message *model = [[Message alloc] initWithEntity:ent insertIntoManagedObjectContext:nil];
                    [model setEntityWithDic:dic];
                    [messages addObject:model];
                }
```
