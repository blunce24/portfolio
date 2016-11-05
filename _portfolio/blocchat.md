---
layout: post
title: BlocChat
thumbnail-path: "img/Index.png"
short-description: BlocChat is a real-time chat application built with AngularJS and Firebase.

---

{:.center}
![]({{ site.baseurl }}/img/Index.png)

## Explanation

[BlocChat](https://github.com/blunce24/bloc-chat) is a real-time chat application built with AngularJS and uses the Firebase Backend-as-a-Service. Working on this application gave me more experience with the Angular framework, as well as front-end programming and debugging techniques.

## Problem

I set out to build a chat application that could send and receive messages in real time, as well as allow users to create their own chatrooms and have their own usernames and accounts. I used the [AngularJS](https://angularjs.org) framework, [Firebase's](https://firebase.google.com) Authentication and Realtime Database, and the [AngularFire API](https://github.com/firebase/angularfire) to construct the chat application, along with [UI Bootstrap](https://angular-ui.github.io/bootstrap/) to create modals and forms. I've highlighted some key challenges and solutions below.

## Solution

**Creating and displaying chat rooms**

* I used AngularFire's `$firebaseArray` service and `$add` method to retrieve and create chatrooms. I used a similar solution to display and create messages.
{% highlight javascript %}
(function() {
  function Room($firebaseArray) {
    var ref = firebase.database().ref().child("rooms");
    var rooms = $firebaseArray(ref);
    return {
      all: rooms,
      create: function(newRoom) {
        rooms.$add(newRoom);
      }
    };
  }

  angular
    .module('blocChat')
    .factory('Room', ['$firebaseArray', Room]);
})();
{% endhighlight %}
* I used a modal with an input form to create a new chat room, using UI Bootstrap's `$uibModal` service.

{:.center}
![]({{ site.baseurl }}/img/Room.png)

**Authentication**

* Authentication modal appears on page load and passes information using AngularFire's `$firebaseAuth` service and `$signInWithEmailAndPassword` method.

{:.center}
![]({{ site.baseurl }}/img/Authentication.png)

* I also added a new account creation and a password recovery features, using AngularFire's `$createUserWithEmailAndPassword` and `$sendPasswordResetEmail`, respectively.

{:.center}
![]({{ site.baseurl }}/img/Create.png)

{:.center}
![]({{ site.baseurl }}/img/Password.png)

## Results

I ran into some interesting debugging challenges during this project. I learned to always spellcheck my code and not to include unnecessary modules. I was able to link input in forms to functions in controllers with `ng-model`, as well as open a modal inside of a modal. In addition, I learned how to use promises to overcome a race condition. This project allowed me to encounter bugs, suffer through them, and come out better on the other side.

## Conclusion

Building the BlocChat application gave me a deeper understanding of how the Angular framework functions, and a taste of back-end programming through Firebase. I am excited to learn more back-end programming to complement my front-end experience so far, and continue my path to become a full-stack software developer. 
