
setTimeout(function(){}, 0); // Släpp event-loopen ett tag.

Enkeltrådat

[[ This javascript is taking too long to respond ]]


// Javaexempel

try {
	User me = TwitterUserSerivce.getUser("warnsberg");
	List<User> followers = 
TwitterUserService.getFollowers(me);
	List<Tweets> tweets = new ArrayList<Tweets>();
	tweets.addAll(TwitterService.loadRecentTweets(followers));
	System.out.println(Joiner.on(", ").join(tweets));
} catch (TwitterException e) {
	System.out.println("Couldn't load tweets :(");
}

setTimeout(function(){}, 0); // Släpp event-loopen ett tag.

twitter.getUser("warnsberg", {
	success: function(user){
		twitter.getFollowers(user, {
			success: function(followers){
  			twitter.loadRecentTweets(followers, {
						success: function(tweets){
							console.log(tweets.join(', '));
						},
						error: function(error){
							console.log('Couldn\'t load tweets :(');
						}
				});
			},
			error: function(error){
				console.log('Couldn\'t load tweets :(');
			}
		});
	},
	error: function(error){
		console.log('Couldn\'t load tweets :(');
	}
});


var twitterError = function(error){
	console.log('Couldn\t load tweets :(');
};

var loadUser = function(){
	twitter.getUser('warnsberg', {
		success: loadFollowers,
		error: twitterError
	});
}

var loadFollowers = function(user){
	twitter.getFollowers(user, {
		success: loadTweets,
		error: twitterError
	});
}

var loadTweets = function(followers){
	twitter.loadRecentTweets(followers, {
		success: printTweets,
	  error: twitterError
	});
};

var printTweets = function(tweets){
	console.log(tweets.join(', '));
}




=======


var twitterError = function(error){
	console.log('Couldn\t load tweets :(');
};

var loadUser = function(username, next){
	if(err) return next(err);
	twitter.getUser(username, next);
}

var loadFollowers = function(err, user, next){
	if(err) return next(err);
	twitter.getFollowers(user, next);
}

var loadTweets = function(err, followers, next){
	if(err) return next(err);
	twitter.loadRecentTweets(followers, next};
});

async.waterfall([loadUser, loadFollowers, loadTweets], 'warnsberg', function(err, tweets){
	if(err) return console.log('Twitter error :(');
	console.log(tweets.join(', '));
});







twitter.getUser('warnsberg').then(function(user){
  return twitter.getFollowers(user);
}).then(function(followers){
  return twitter.loadRecentTweets(followers);
}).then(function(tweets){
  console.log(tweets.join(', '));
}, function(error){
  console.log('Couldn\'t load tweets :(');
});


, {
  success: function(user){
    twitter.getFollowers(user, {
      success: function(followers){
        twitter.loadRecentTweets(followers, {
            success: function(tweets){
              console.log(tweets.join(', '));
            },
            error: function(error){
              console.log('Couldn\'t load tweets :(');
            }
        });
      },
      error: function(error){
        console.log('Couldn\'t load tweets :(');
      }
    });
  },
  error: function(error){
    console.log('Couldn\'t load tweets :(');
  }
});





´this´


alert(this.name); // this === window, or the global context.



$('a').click(function(){
  alert(this); // this === the element that fired the event
});



var obj = {
  name: 'An object',
  greet: function(greeting){
    alert(greeting + this.name);
  }
};

obj.geet('Hello, ');


var obj = {
  name: 'An object',
  init: function(){
    var self = this;
    var test = function(){
      alert(this.name); // this === window
      alert(self.name);
    }
    test();
  }
};

obj.init();


var obj = {
  name: "A name",
  test: function(greeting){ alert(greeting + this.name); }
}

var stolen = obj.test;
stolen("Hello, "); // window.name


Function.prototype.call(context, arg1, arg2, ...argN);

Function.prototype.apply(context, [args]);


stolen.call(obj, "Bonjour, ");

stolen.apply(obj, ["Guten tag, "]);


Function.prototype.bind = function(context){
  var __func = this,
      slice  = [].slice,
      args   = slice.call(arguments, 1);
  return function(){
    args = args.concat(slice.call(arguments, 0));
    __func.apply(context, args);
  };
};

stolen.bind(obj).greet('Hello, ');
