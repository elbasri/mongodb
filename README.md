# Mapreduce on mongodb documents (javascript syntaxed script)

## *Map function*:

```javascript
var map = function() {  
  //this.title is the document title and its will be affected to the new variable title below
    var title = this.title;
    
    //check if title is not null
    if (title) { 
      //convert all upercase chars to lowercase chars in the title value
        title = title.toLowerCase().split(" "); 
        
        //loop all chars and substrings contained in the title string
        for (var i = title.length - 1; i >= 0; i--) {
          //check the substring lenght... if it is greater than 4 then emit title string with the value 1.
            if (title[i] && title[i].length>4)  {
               emit(title[i], 1);
            }
        }
    }
};

```

## *Reduce function*:

```javascript
var reduce = function( key, values ) {    
  //initiate a count variable
    var count = 0;    
    //loop all values emited at Map level
    values.forEach(function(v) {  
      //increment the count variable by "v" value (its 1 in this example)
        count +=v;    
    });
    //return the finale count value
    return count;
}

```

## *Execute the functions Map and Reduce created above*:

```json
#the functions will be executed on a collection called "news" and return counts of all words having length greater than 4 chars, then put the result in a new collection called "word_count":

db.news.mapReduce(map, reduce, { out: "word_count" })

#then, we can query this new collection to show the words with its counts, example:
db.word_count.find().sort({ value: -1 }).limit(10)


```
## *Real example applied on Kachaf.com project under this app called "wolrd maps news"*:

https://www.kachaf.com/worldmapnews.php

Credits to https://wwww.elbasri.net
