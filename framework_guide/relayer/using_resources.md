# Using Resources

Ok, you've setup an API server, you've defined some resources on the client, how do you actually use them? This is where the fun comes in. The relayer interface for loading resources is extremely simple. Let's say I have an angular controller function, that takes in my resources API as an injected dependency. Here is how you would actually load blog pages, authors and comments from the example above

function MyController($scope, resources) {
  resources.blogPosts({id: 1}).comments().load((comments) => {
    $scope.comments = comments;
  });
}
MyController.$inject = ["$scope", "resources"]

or loading an author's blog posts:

resources.authors({nickname: "karlm").blogPosts().load((blogPosts) => {
  $scope.blogPosts = blogPosts;
});


*** load will execute a callback if one is given, otherwise it will return a promise ***

What about saving? For updating existing resources, it's very easy:

$scope.comments.update();
$scope.blogPosts.update();

If you have new resources, you'll need to do a POST:

var blogPost = $scope.blogPosts.new();
blogPost.body = "The production of too many useful things results in too many useless people."
$scope.blogPosts.create(blogPost)

All of the traversing of API's is done behind the scenes. Importantly, you don't actually have to even know if a related resource is linked or embedded in the JSON response that comes back to the server -- relayer will work with what you get and navigate as neccesary.
