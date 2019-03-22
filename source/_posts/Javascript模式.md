---
title: Javascript模式
date: 2019-03-21 17:25:37
categaries: web前端
tags:
- web前端
- javascript
---

### 单例模式
```js
function Universe() {
    //缓存实例
    var instance;
    //重写构造函数
    Universe = function Universe() {
      return instance;
    };
    //保留原型属性
    Universe.prototype = this;
    //实例
    instance = new Universe();
    //重置构造函数指针
    instance.constructor = Universe;
    //所有功能
    instance. start_time =0;
    instance.bang = "Big";
    return instance;
}
```
#### 另一种写法
```js
var Universe;
(function() {
    var instance;
    Universe = function Universe() {
      if (instance) {
        return instance;
      }
      instance = this;
    
      this.start_time = 0;
      this.bang = "Big";
    }
}());
```

### 工厂模式

```js
//父构造函数
function CarMaker() { }
// a method of the parent
CarMaker.prototype.drive = function () {
  return "Vroom, I have " + this.doors + " doors";
};
//静态工厂方法
CarMaker.factory = function (type) {
var constr = type,
  newcar;
//如果构造函数不存在,则发生错误
if (typeof CarMaker[constr] !== "function") {
  throw {
    name: "Error",
    message: constr + "doesn’t exist"
  };
}
if(typeof CarMaker[constr].prototype.drive !== "function") {
  CarMaker[constr].prototype = new CarMaker();
}
newcar = new CarMaker[constr]();
  return newcar;
};
CarMaker.Compact = function() {
  this.doors = 4;
}
CarMaker.Convertible = function() {
  this.doors = 2;
}
CarMaker.SUV = function() {
  this.doors = 24;
}
```

### 迭代模式

```js
var agg = (function () {
    var index = 0,
      data = [1, 2, 3, 4, 5],
      length = data.length;
    return {
      next: function () {
        var element;
        if (!this.hasNext()) {
          return null;
        }
    
        element = data[index];
        index = index + 2;
        return element;
      },
      hasNext: function() {
        return index < length;
      },
      rewind: function() {
        index = 0;
      },
      current: function() {
        return data[index];
      }
    }
}());
```

### 装饰器模式

```js
function Sale(price) {
    this.price = price || 100;
  }
  Sale.prototype.getPrice = function() {
    return this.price;
  }
  Sale.decorators = {};
  Sale.decorators.fedtax = {
    getPrice: function() {
      var price = this.uber.getPrice();
      price += price * 5 / 100;
      return price;
    }
  };
  Sale.decorators.quebec = {
    getPrice: function() {
      var price = this.uber.getPrice();
      price += price * 7.5 / 100;
      return price;
    }
  };
  Sale.decorators.money = {
    getPrice: function() {
      return "$" + this.uber.getPrice().toFixed(2);
    }
  };
  Sale.decorators.cdn = {
    getPrice: function() {
      return "CDN$ " + this.uber.getPrice().toFixed(2);
    }
  };
  Sale.prototype.decorate = function(decorator) {
    var F = function() {},
      overrides = this.constructor.decorators[decorator],
      i, newobj;
    F.prototype = this;
    newobj = new F();
    newobj.uber = F.prototype;
    for (i in overrides) {
      if (overrides.hasOwnProperty(i)) {
        newobj[i] = overrides[i];
      }
    }
    return newobj;
  }
```
### 策略模式

```js
var validator = {
    types: {},
    messages: [],
    config: {},
    validate: function(data) {
      var i, msg, type, checker, result_ok;
      this.messages = [];
      for (i in data) {
        if (data.hasOwnProperty(i)) {
          type = this.config[i];
          checker = this.types[type];

          if(!type) {
            continue;
          }
          if (!checker) {
            throw {
              name: "ValidationError",
              messages: "No handler to validate type " + type
            };
          }
          result_ok = checker.validate(data[i]);
          if (!result_ok) {
            msg = "Invalid value for *" + i + "*, " + checker.instructions;
            this.messages.push(msg);
          }
        }
      }
      return this.hasErrors();
    },
    hasErrors() {
      return this.messages.length !== 0;
    }
  }
  validator.types.isNonEmpty = {
    validate: function(value) {
      return value !== "";
    },
    instructions: "the value cannot be empty"
  };
  validator.types.isNumber = {
    validate: function(value) {
      return !isNaN(value);
    },
    instructions: "the value can only be a valid number, e.g. 1, 3.14 or 2010"
  };
  validator.types.isAlphaNum = {
    validate: function(value) {
      return !/[^a-z0-9]/i.test(value);
    },
    instructions: "the value can only contain characters and numbers, no special symbols"
  };
  validator.config = {
    first_name: "isNonEmpty",
    age: "isNumber",
    username: "isAlphaNum",
  }
  var data = {
    first_name: "super",
    last_name: "Man",
    age: "unknown",
    username: "o_0"
  }
  validator.validate(data);
  if (validator.hasErrors()) {
    console.log(validator.messages.join("\n"));
  }

```

### 外观模式

```js
var myevent = {
  stop: function(e) {
    if (typeof e.preventDefault === 'function') {
      e.preventDefault();
    }
    if (typeof e.stopPropagation === 'function') {
      e.stopPropagation();
    }
    if (typeof e.returnValue === 'boolean') {
      e.returnValue = false
    }
    if (typeof e.cancelBubble === 'boolean') {
      e.cancelBubble = true;
    }
  }
}
```

### 代理模式
> 作用：合并多个请求

```js
var proxy = {
  ids: [],
  delay: 50,
  timeout: null,
  callback: null,
  context: null,
  makeRequest: function(id, callback, context) {
    this.ids.push(id);
    this.callback = callback;
    this.context = context;

    if(!this.timeout) {
      this.timeout = setTimeout(function() {
        proxy.flush();
      }, this.delay);
    }
  },
  flush: function() {
    http.makeRequest(this.ids, "proxy.handler");
    this.timeout = null;
    this.ids = [];
  },
  handler: function(data) {
    var i, max;
    if(parseInt(data.query.count, 10) === 1) {
      proxy.callback.call(proxy.context, data.query.results.Video);
      return;
    }
    for (i = 0, max = data.query.results.Video.length; i < max; i += 1) {
      proxy.callback.call(proxy.context, data.query.results.Video[i]);
    }
  }
};

```

### 中介者模式

```js
// player constructor
function Player(name) {
    this.points = 0;
    this.name = name;
}
// play method
Player.prototype.play = function () {
    this.points += 1;
    mediator.played();
};

// the scoreboard object
var scoreboard = {
    
    // HTML element to be updated
    element: document.getElementById('results'),
    
    // update the score display
    update: function (score) {
        
        var i, msg = '';
        for (i in score) {
            if (score.hasOwnProperty(i)) {
                msg += '<p><strong>' + i + '<\/strong>: ';
                msg += score[i];
                msg += '<\/p>';
            }
        }
        this.element.innerHTML = msg;
    }
};


var mediator = {
    
    // all the players
    players: {},
    
    // 
    setup: function () {
        var players = this.players;
        players.home = new Player('Home');
        players.guest = new Player('Guest');
        
    },
    
    // someone plays, update the score
    played: function () {
        var players = this.players,
            score = {
                Home:  players.home.points,
                Guest: players.guest.points
            };
            
        scoreboard.update(score);
    },
    
    // handle user interactions
    keypress: function (e) {
        e = e || window.event; // IE
        if (e.which === 49) { // key "1"
            mediator.players.home.play();
            return;
        }
        if (e.which === 48) { // key "0"
            mediator.players.guest.play();
            return;
        }
    }
};


// go!
mediator.setup();
window.onkeypress = mediator.keypress;

// game over in 30 seconds
setTimeout(function () {
    window.onkeypress = null;
    alert('Game over!');
}, 30000);
```

### 观察者模式

```js
var publisher = {
    subscribers: {
        any: [] // event type: subscribers
    },
    subscribe: function (fn, type) {
        type = type || 'any';
        if (typeof this.subscribers[type] === "undefined") {
            this.subscribers[type] = [];
        }
        this.subscribers[type].push(fn);
    },
    unsubscribe: function (fn, type) {
        this.visitSubscribers('unsubscribe', fn, type);
    },
    publish: function (publication, type) {
        this.visitSubscribers('publish', publication, type);
    },
    visitSubscribers: function (action, arg, type) {
        var pubtype = type || 'any',
            subscribers = this.subscribers[pubtype],
            i,
            max = subscribers.length;
            
        for (i = 0; i < max; i += 1) {
            if (action === 'publish') {
                subscribers[i](arg);
            } else {
                if (subscribers[i] === arg) {
                    subscribers.splice(i, 1);
                }
            }
        }
    }
};

/*
var s1 = {log: console.log},
    s2 = {err: console.error},
    s3 = {warn: console.warn};


publisher.subscribe(s1.log);
publisher.subscribe(s2.err);
publisher.subscribe(s3.warn);

publisher.publish({hello: "World"});

publisher.unsubscribe(s2.err);
publisher.publish("hello");


publisher.subscribe(s1.log, "log");
publisher.publish({obj: "log this object"}, "log");
*/

function makePublisher(o) {
    var i;
    for (i in publisher) {
        if (publisher.hasOwnProperty(i) && typeof publisher[i] === "function") {
            o[i] = publisher[i];
        }
    }
    o.subscribers = {any: []};
}

var paper = {
    daily: function () {
        this.publish("big news today");
    },
    monthly: function () {
        this.publish("interesting analysis", "monthly");
    }
};

makePublisher(paper);

var joe = {
    drinkCoffee: function (paper) {
        console.log('Just read ' + paper);
    },
    sundayPreNap: function (monthly) {
        console.log('About to fall asleep reading this ' + monthly);
    }
};

paper.subscribe(joe.drinkCoffee);
paper.subscribe(joe.sundayPreNap, 'monthly');

paper.daily();
paper.daily();
paper.daily();
paper.monthly();


makePublisher(joe);

joe.tweet = function (msg) {
    this.publish(msg);
};

paper.readTweets = function (tweet) {
    alert('Call big meeting! Someone ' + tweet);
};

joe.subscribe(paper.readTweets);

joe.tweet("hated the paper today");
```


```js
var publisher = {
    subscribers: {
        any: []
    },
    on: function (type, fn, context) {
        type = type || 'any';
        fn = typeof fn === "function" ? fn : context[fn];
        
        if (typeof this.subscribers[type] === "undefined") {
            this.subscribers[type] = [];
        }
        this.subscribers[type].push({fn: fn, context: context || this});
    },
    remove: function (type, fn, context) {
        this.visitSubscribers('unsubscribe', type, fn, context);
    },
    fire: function (type, publication) {
        this.visitSubscribers('publish', type, publication);
    },
    visitSubscribers: function (action, type, arg, context) {
        var pubtype = type || 'any',
            subscribers = this.subscribers[pubtype],
            i,
            max = subscribers ? subscribers.length : 0;
            
        for (i = 0; i < max; i += 1) {
            if (action === 'publish') {
                subscribers[i].fn.call(subscribers[i].context, arg);
            } else {
                if (subscribers[i].fn === arg && subscribers[i].context === context) {
                    subscribers.splice(i, 1);
                }
            }
        }
    }
};


function makePublisher(o) {
    var i;
    for (i in publisher) {
        if (publisher.hasOwnProperty(i) && typeof publisher[i] === "function") {
            o[i] = publisher[i];
        }
    }
    o.subscribers = {any: []};
}

var game = {
    
    keys: {},

    addPlayer: function (player) {
        var key = player.key.toString().charCodeAt(0);
        this.keys[key] = player;
    },

    handleKeypress: function (e) {
        e = e || window.event; // IE
        if (game.keys[e.which]) {
            game.keys[e.which].play();
        }
    },
    
    handlePlay: function (player) {
        var i, 
            players = this.keys,
            score = {};
        
        for (i in players) {
            if (players.hasOwnProperty(i)) {
                score[players[i].name] = players[i].points;
            }
        }
        this.fire('scorechange', score);
    }
};

function Player(name, key) {
    this.points = 0;
    this.name = name;
    this.key  = key;
    this.fire('newplayer', this);
}

Player.prototype.play = function () {
    this.points += 1;
    this.fire('play', this);
};

var scoreboard = {
    
    element: document.getElementById('results'),
    
    update: function (score) {
        
        var i, msg = '';
        for (i in score) {
            if (score.hasOwnProperty(i)) {
                msg += '<p><strong>' + i + '<\/strong>: ';
                msg += score[i];
                msg += '<\/p>';
            }
        }
        this.element.innerHTML = msg;
    }
};


makePublisher(Player.prototype);
makePublisher(game);

Player.prototype.on("newplayer", "addPlayer", game);
Player.prototype.on("play",      "handlePlay", game);

game.on("scorechange", scoreboard.update, scoreboard);

window.onkeypress = game.handleKeypress;


var playername, key;
while (1) {
    playername = prompt("Add player (name)");
    if (!playername) {
        break;
    }
    while (1) {
        key = prompt("Key for " + playername + "?");
        if (key) {
            break;
        }
    }
    new Player(playername,  key);    
}


```
