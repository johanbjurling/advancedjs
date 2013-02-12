
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

var loadUser = function(err, next){
	if(err) return next(err);
	twitter.getUser('warnsberg', {
		success: next.bind(this, null),
		error: next
	});
}

var loadFollowers = function(err, user, next){
	if(err) return next(err);
	twitter.getFollowers(user, {
		success: next.bind(this, null),
		error: next
	});
}

var loadTweets = function(err, followers, next){
	if(err) return next(err);
	twitter.loadRecentTweets(followers, {
		success: next.bind(this, null),
	  error: next
	});
};

async.waterfall([loadUser, loadFollowers, loadTweets], function(err, tweets){
	if(err) return console.log('Twitter error :(');
	console.log(tweets.join(', '));
});