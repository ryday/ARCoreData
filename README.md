![](https://github.com/AugustRush/ARCoreData/blob/master/6DB73380-0D9A-43A1-AD21-8374D748429A.png)

* ARCoreData is a library to make CoreData easily. we can become a good friend with CoreData now. Don't need any config，just enjoy a high performance object storage。

*****************************************

## Features

###### PrimaryKey

###### Auto Mapping (attribute，relationship)

###### Safety Mutlie thread

###### Async fetch

###### ARTableViewFetchResultController(a convenience class to replace NSFetchResultControler for UITableView)

###### ARCollectionViewFetchResultController(a convenience class to replace NSFetchResultControler for UICollectionView)

## Install

#### Manualy
just drag ARCoreData to your project and edit you model , do not need to config any others 
<br>Import: `#import "ARCoreData.h"`

#### Cocoapods
* pod 'ARCoreData', :git => 'https://github.com/AugustRush/ARCoreData.git'

## Getting start
***********************

## Creat new object

```
/**
 *  creat an entity in mainQueue context
 *
 *  @return entity
 */
+(id)AR_new;

/**
 *  creat an new entity in your context
 *
 *  @param context your context
 *
 *  @return entity
 */
+(id)AR_newInContext:(NSManagedObjectContext *)context;

/**
 *  to ceate new or update existed object with JSON, this class should impliment ARManageObjectMappingProtocol protocol
 *
 *  @param JSON key value object(KVC object)
 *
 *  @return mapping object
 */
+(id)AR_newOrUpdateWithJSON:(id)JSON;
/**
 *  to ceate new or update existed objects with JSONs, this class should impliment ARManageObjectMappingProtocol protocol
 *
 *  @param JSON key value objects(KVC objects)
 *
 *  @return mapping objects
 */
+(NSArray *)AR_newOrUpdateWithJSONs:(NSArray *)JSONs;

/**
 *  to ceate new or update existed object with JSON, this class should impliment ARManageObjectMappingProtocol protocol
 *
 *  @param JSON   JSON key value object(KVC object)
 *  @param policy ARRelationshipMergePolicy custom
 *
 *  @return mapping object
 */
+(id)AR_newOrUpdateWithJSON:(id)JSON relationshipMergePolicy:(ARRelationshipMergePolicy)policy;

/**
 *  to ceate new or update existed objects with JSONs, this class should impliment ARManageObjectMappingProtocol protocol
 *
 *  @param JSONs  JSON key value objects(KVC objects)
 *  @param policy ARRelationshipMergePolicy custom
 *
 *  @return mapping objects
 */
+(NSArray *)AR_newOrUpdateWithJSONs:(NSArray *)JSONs relationshipsMergePolicy:(ARRelationshipMergePolicy)policy;

```
## Mapping

```
@protocol ARManageObjectMappingProtocol <NSObject>

+(NSDictionary *)JSONKeyPathsByPropertyKey;

@optional
+(NSString *)primaryKey;

```

if you implement the class method +(NSString *)primaryKey; you just can create an unique person through a same "guid".

I have implemented some methods, you can use server response directly to create an(or a array) manageObject(s),
there have two methods :

```
+(id)AR_newOrUpdateWithJSON:(NSDictionary *)JSON;

+(NSArray *)AR_newOrUpdateWithJSONs:(NSArray *)JSONs;

```
you have seen [ARManageObjectMappingProtocol](https://github.com/AugustRush/ARCoreData/blob/master/ARCoreData/Core/ARManageObjectMappingProtocol.h) , yes ,this protocol have two methods,like famous mapping library <a href="https://github.com/Mantle/Mantle">Mantle</a>, but it must be faster than Mantle. you just to impliment these two methods , and just use two methods the above, it will automaticly transfrom a JSON(s) or KVC object(s) to NSManageObject(s) instance. Overall, it's very easy and safe.

#### Note: you can create an custom mapping for you coreData model's attribute or relationships like Mantle. the detail in my demo.

## Fetch objects

there have a lot of methods to help you fetch objects convenience and faster, you can see the file [Fetch](https://github.com/AugustRush/ARCoreData/blob/master/ARCoreData/Core/NSManagedObject%2BARConvenience.h).

Example:
```
    NSArray *allPersons = [Person AR_all];
    
    NSArray *persons = [Person AR_where:@"name = %@",@"a name"];
    
    NSArray *persons = [Person AR_whereProperty:@"guid" equalTo:@3];
```
and so on !!!

## Saving objects

```
* sync
    [Person AR_saveAndWait];

    [Person AR_saveAndWaitCompletion:^(BOOL success, NSError *error) {
        // fetch objects or do UI work
    }];

* async

	[Person AR_saveCompletion:^(BOOL success, NSError *error) {
        NSLog(@"all dog is %@ dog count is %ld",[Dog AR_all],[Dog AR_count]);
        
        NSLog(@"all person is %@ dog count is %ld",[Person AR_all],[Person AR_count]);
    }];


```
## TODO:

* Migration

* Encryption

* Fixed bugs

## TL;DR:
there have more methods i have created for you, you can see it in my Demo project after. this library also worked in
swift fine !




