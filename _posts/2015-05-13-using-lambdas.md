---
title: Using Lambdas Effectively in Java 
updated: 2016-01-28 22:43
---

One of the most powerful features python offers is lambdas which allow you to perform operations on a collection and create anonymous functions on the data set to filter, map (transform) or reduce.  Here's a snippet of code that outlines converting a complex meta data structure filtering on a repeating group and extracting out the position and sorting it and converting it to a displayable list.

```python 
metas = get_rows('META')
definition_row = filter(lambda r: get_str_val(r, 'display') == table_name, metas)[0]
var_groups = definition_row['data']['LAYOUT'].values()
var_groups = filter(lambda var_group: 'position' in var_group, var_groups) 
var_groups = sorted(var_groups, key=lambda x: x['position']) 
header_cap_ids = map(lambda var_group: var_group['display'], var_groups) 
```

Now if we're planning to do this in Java, it will be slightly more complicated and verbose.  Without having Java 8 at our disposal (restricted by our app server version deployment), we're limited to other frameworks (primarily LambaJ and Guava).  LambaJ proved to be a little cumbersome and felt too integrated to the ORM model (similar to LINQ query) so Guava proved to be a better alternative.  It is still a little verbose with with new anonymous classes being created per operation but it gets the gist what needs to be done.

```java
 DataRow row = CollectionUtils.getFirst(ThriftDBUtils.getAllRows("META"));
 List<DataGroup> groups = ThriftDataUtils.getGroups(row, "CONTENT", "LAYOUT");
 List<DataGroup> filter = CollectionUtils.filter(groups, new Predicate<DataGroup>()
 {
  @Override
  public boolean apply(DataGroup group)
  {
   String stringValue = ThriftDataUtils.getStringValue(group, "position");
   return !StringUtils.isEmpty(stringValue);
  }
 });
 CollectionUtils.sort(filter, new Comparator<DataGroup>()
 {
  @Override
  public int compare(DataGroup group1, DataGroup group2)
  {
   long long1 = Long.parseLong(ThriftDataUtils.getStringValue(group1, "position"));
   long long2 = Long.parseLong(ThriftDataUtils.getStringValue(group2, "position"));
   return ComparisonChain.start()
    .compare(long1, long2)
    .result();
  }
 });
 List<String> transform = CollectionUtils.transform(filter, new Function<DataGroup, String>()
 {
  @Override
  public String apply(DataGroup group)
  {
   String stringValue = ThriftDataUtils.getStringValue(group, "display");
   return stringValue;
  }
 });
```

There is a word of caution submitted by the Guava team on excessive use of lambdas which can lead to confusing, unreadable and inefficient code when a simple iterative function will do.  Concerning ourselves, I find it useful when performing series of operations that will change frequency and deals with converting or filtering on a complex data set.