---
layout: post
title: Debugging Is Life
---

I've finished my second front-end project, BlocChat, a real-time chat application built with AngularJS and Firebase. During this project, I had to go through a lot of debugging, which is both a great and an awful thing.

Debugging can be frustrating and demoralizing, but it's also the best way to learn. During the BlocChat project, for example, I was able to learn about race conditions and promises as a way to debug my application. I was trying to set the `currentRoom` variable in a controller, but I wasn't able to due to a race condition. I was able to use a promise, detailed below, to circumvent this and obtain the desired outcome.
{% highlight javascript %}
rooms.$loaded().then(function(rooms) {
           var key1 = '-KUt6u7dYNBLuSKCOMG7';
           room = rooms.$getRecord(key1);
           $scope.currentRoom = room;
           $scope.messages = Message.getByRoomId($scope.currentRoom.$id);
});
{% endhighlight %}

My least favorite, and most frustrating, debugging experiences are when I have a spelling error, or other equally stupid mistake. You quickly learn which parts of the application are most susceptible to these mistakes, and check these constantly.
