# OLCOrm v0.0.6

[![CI Status](http://img.shields.io/travis/Lakitha Samarasinghe/OLCOrm.svg?style=flat)](https://travis-ci.org/Lakitha Samarasinghe/OLCOrm)
[![Version](https://img.shields.io/cocoapods/v/OLCOrm.svg?style=flat)](http://cocoadocs.org/docsets/OLCOrm)
[![License](https://img.shields.io/cocoapods/l/OLCOrm.svg?style=flat)](http://cocoadocs.org/docsets/OLCOrm)
[![Platform](https://img.shields.io/cocoapods/p/OLCOrm.svg?style=flat)](http://cocoadocs.org/docsets/OLCOrm)

## Usage

To run the example project, clone the repo, and run `pod install` from the Example directory first.

## Requirements

## Dependencies

FMDB - https://github.com/ccgus/fmdb

    pod "FMDB"

## Installation

OLCOrm is available through [CocoaPods](http://cocoapods.org). To install
it, simply add the following line to your Podfile:

    pod "OLCOrm"
    
## What's new ?

2014-08-19

> Basic relationship mapping added to the lib. One-to-One and One-to-Many relations.

2014-09-15

> Now truncating tables are possible. This will reset the auto incremental id value as well.

2014-09-29

> Lib updated to prevent sql injection.

2014-10-14

> NSDate failing to retrieve from database issue fixed.

## Usage

There are two main classes in OLCOrm

1. 'OLCMigrator' - which handles the the database and table create part.
2. 'OCLModel' - Model class that you need to extend your model classes from.

** Creating the database **

import the "OLCMigrator.h" class first.

Creating a database is pretty straightforward, all you need to do is add this line to your AppDelegate.m file, at 'didFinishLaunchingWithOptions' method.

    OLCMigrator *dbH = [OLCMigrator sharedInstance:@"olcdemo.sqlite" version:[NSNumber numberWithInt:1] enableDebug:NO];
    [dbH initDb];
    
here 'version' is the database version that you must manage. Normaly you will need to update this as you update your model classes. *WARNING: When you update your model and run the app again, it will drop all tables from the database and add them back with updates.
    
** Creating the Tables **

After you add the code to create the database, add this line to create the table.

    [dbH makeTable:[TestObject class]];
    
Or

    [dbH makeTable:[[TestObject class] ] withTableVersion:[NSNumber numberWithInt:1]]
    
First option is the strait foward way of creating a table and let the database init code to handle the table versioning part. But you can always maitain the table versioning manually like this.

** Inserting a Record **

    TestObject *test = [[TestObject alloc] init];
    
    test.title = [NSString stringWithFormat:@"Sample Record %d", [records count]];
    test.coordinates = [NSNumber numberWithDouble:234.345345];
    test.currency = [NSNumber numberWithFloat:150.00];
    test.flag = [NSNumber numberWithInt:1];
    test.addAt = [NSDate date];
    test.link = [NSURL URLWithString:@"http://google.com"];
    test.status = [NSNumber numberWithInt:1];
    
    return [test save];
    
** Update a Record **

    test.title = @"Updated title";
    [test update];
    
** Delete Record **

    [test delete];
    
** Querying the Database **

    NSArray *allRecords = [TestObject all];
    
    TestObject *findObj = [TestObject find:[NSNumber numberWithInt:1]];
    
    NSArray *searchObjs = [TestObject whereColumn:@"search_column" byOperator:@">=" forValue:@"4"];
    
    NSArray *searchObjs = [TestObject where:@"column_1 = 2 AND column_2 > 12" sortBy:@"column_1 ASC"];
    
    NSArray *searchObjs = [TestObject query:@"SELECT * FROM myTable WHERE STATUS = 1"];
    
** Working with Relationships **

One-to-One

    [self hasOne:<Model class> foreignKeyCol:<Foreign Key> primaryKeyCol:<Primary Key>]
    
Exsample:

Add this method to your model class
    
    - (UserObject *) hasUser
    {
        return (UserObject*) [self hasOne:[UserObject class] foreignKeyCol:@"userId" primaryKeyCol:@"Id"];
    }
    
One-to-Many

    [self hasMany:<Model class> foreignKeyCol:<Foreign Key> primaryKeyCol:<Primary Key>];
    
Exsample:

Add this method to your model class
    
    - (NSArray *) hasTests
    {
        return [self hasMany:[TestObject class] foreignKeyCol:@"userId" primaryKeyCol:@"Id"];
    }

## Special Thanks

Special thanks goes to 'ccgus' for the awesome FMDB database warpper library. And to 'kevinejohn' for his NSObject-KJSerializer (https://github.com/kevinejohn/NSObject-KJSerializer) utlity class.

## Note

We all know the working with Core Data is major pain in the neck. Yet it is in a way bit easy you work with, if you get an hold of how it actaully work, working with only model calsses to update the database structre on the fly.

And for those who hate to use Core Data, FMDB is you best choice. But still it lack the capability of mapping objects to models. So you  have to manullay create the databas and queries, ah.

So I develop this Libaray as an wrapper library to FMDB, that handle the database, table & all other CRUD function that we use daily. To make you life easire.

This libaray is still at it early stages (not even Alpha) so you are always welcome to teak thigs here and there to make this better :).
                       
Happy Coding.

## Author

Lakitha Samarasinghe, lakitharav@gmail.com

## License

OLCOrm is available under the MIT license. See the LICENSE file for more info.

