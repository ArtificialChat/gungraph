# All about souls

And a summary of gun's graph architecture

But first..

# Why gun?
It's a realtime, decentralized, offline-first, graph database that's as fast as light. 

Android phone, ~5M ops/sec.

Macbook Air, Chrome, ~30M ops/sec.

Macbook Pro, Chrome Canary, ~80M ops/sec.

Lenovo netbook, IE6, ~100K ops/sec.

Compare to Redis at 0.5M ops/sec (cached reads, Macbook Air), even with pipeline optimizations turned on, here: https://redis.io/topics/benchmarks .

# What's a Graph db, really?

There are a lot of competing database structures out there, like document, tree, tabular, relational... it just seems to go on. But there is one structure that rules them all -- the graph. With graphs, you can make any other data structure, and it's at the heart of gun's paradigm. Here's how it works:

You start with the root object
```javascript
{

}
```
then you fill that object with other named objects
```javascript
{
  bob: {},
  joe: {},
  dave: {}
}
```
but the structure never goes any deeper than that. If you want to add an object inside of another object, you name that object and simply reference it
```javascript
{
  bob: {
    friend: { name: joe }
  },
  joe: {
    friend: { name: sam }
  },
  sam: {
    friend: { name: 'pizza' }
  }
}
```
you'll never actually see names like these in gun, because they're human generated and horribly difficult to come up with. These are computers we're talking about. Instead, we let the computers come up with super long, randomly generated names. That way, we never have to worry about naming conflicts and all of our objects are uniquely identifiable

<!-- TODO: cover relations and what they do -->
```javascript
{
  'ckxAzmBrErR1': {
    friend: { name: 'qx8QuUBturo8' }
  },
  'qx8QuUBturo8': {
    friend: { name: 'u12wN9znw50y' }
  },
  'u12wN9znw50y': {
    friend: { name: 'pizza' }
  }
}
```
Now we can structure our data in whatever pattern we want without hitting complicated nesting issues, because we're just keeping a *reference* to that object, not the object itself. In gun, we refer to these programmatically generated names as "souls", and they drive the entire system. The reason we wrap them inside objects is so that we can tell them apart from ordinary strings. We call those objects "relations", because they "relate" or point to other objects. We don't actually call the property `"name"`, in gun we use a special symbol: the hash sign. Relations look something like this: `{ '#': 'aOCLhos5ADx3' }`. So whenever you see that weird-looking object, it really just means "link to this other object".

So, to recap:
- in a graph, objects never contain other objects, just *references*
- gun calls those references "souls"
- souls wrapped in objects with the hash symbol are called "relations"
- graphs can form any data structure
- and are crazy cool

That should give you an idea of how gun's graph structure works and what exactly a soul is.

#### [Now make a Graph DB!]( https://artificialchat.github.io/gungraph/)


Thank you Gun-Scape!

<img src="http://www.cytoscape.org/images/logo/cy3logoOrange.svg" height=50>

# Gun-scape 
[GunDB](https://github.com/amark/gun) Cytoscape Graph Visualizer + Live Editor

#### [Try the Live Demo](http://rawgit.com/lmangani/gun-scape/master/index.html)
Execute your own _"Shot-GunDB Wedding"_ and test all Gun [commands](http://gun.js.org/docs/get.html) in a snap:
![ezgif com-optimize 30](https://user-images.githubusercontent.com/1423657/31863000-bdd31120-b747-11e7-8b7c-b2586aa7cae8.gif)
<!-- ![ezgif com-optimize 29](https://user-images.githubusercontent.com/1423657/31855811-136fc9e2-b6b3-11e7-9b40-0b6e1a57ad29.gif) -->

###### Flow Example
```
g.get('ua').put({name: 'SIP User-Agent'});
g.get('opensips').put({name: 'OpenSIPS'});
g.get('asterisk').put({name: 'Asterisk'});
g.get('rtpengine').put({name: 'RTP:Engine'});
g.get('homer').put({name: 'HOMER'});
g.get('ua').get('sip').put(g.get('opensips')); 
g.get('ua').get('rtp').put(g.get('rtpengine')); 
g.get('opensips').get('sip').put(g.get('asterisk')); 
g.get('rtpengine').get('rtp').put(g.get('asterisk')); 
g.get('opensips').get('hep-sip').put(g.get('homer')); 
g.get('asterisk').get('hep-sip').put(g.get('homer')); 
g.get('rtpengine').get('hep-rtcp').put(g.get('homer')); 
```
<img src="https://user-images.githubusercontent.com/1423657/31861056-e283d9f4-b725-11e7-8e77-faddca3bbaf8.png" width=500/>

### Credit
This Demo is based on [serve-gun](https://github.com/JosePedroDias/serve-gundb)
