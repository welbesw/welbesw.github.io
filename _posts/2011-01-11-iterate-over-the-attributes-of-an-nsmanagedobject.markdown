---
layout: post
title:  "Iterate over the attributes of an NSManagedObject"
date:   2011-01-11 11:00:00
tags: iOS
---

Given an NSManagedObject subclass that has several attributes defined as properties - a class file created from the CoreData model - how can one determine if the NSString attributes are "empty"?

Rather than write code that checks each property on the object individually, I came up with the following method to iterate over the attributes of an NSManagedObject subclass: 

```obj-c
//Define a variable to determine if all string attributes are blank
BOOL isBlank = YES;

//Get a reference to the entity description for the NSManagedObject subclass - CoreData created entity
NSEntityDescription * myEntity = [managedObjectSubClass entity];

//Get all of the attributes that are defined for the entity - not the relationship properties - just attributes
NSDictionary * attributes = [myEntity attributesByName];

//Loop over the attributes by name  
for (NSString * attributeName in [attributes allKeys]) {
 
 //Determine if this property is a string
 SEL selector = NSSelectorFromString(attributeName);
 id attributeValue = [managedObjectSubClass performSelector:selector];

 if ([attributeValue isKindOfClass:[NSString class]]) {
  //Check to see if the string is empty
  isBlank &= [((NSString*)attributeValue) isEmpty];
 }
}

```

isEmpty is a category on NSString defined as follows:

```obj-c
@implementation NSString (NSString_Additions)

- (BOOL) isEmpty{
 NSString * trimmedString = [self stringByTrimmingCharactersInSet:
 	[NSCharacterSet whitespaceAndNewlineCharacterSet]];
 return [trimmedString length] == 0;
}

```

@end