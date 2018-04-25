# Video Script

[Showing the speakr full screen]

Hello and welcome to our X episode of ProdOps talks.

My name is Omer, and today we'll be talking about Docker basics.

So, first thing's first -> What's Docker?

Docker is the most common containerization platform in the world.

"A container is a small poackage that container the entire requirements to run a program
It's lighter, faster, uses smart mechanisms for caching and building new containers in "layers". 
Layers can be shared among containers, and are used to save space and time when interacting with containers,
whether by build / ship / pull / push / run etc.."

Ok, so why should developers care about all this operational stuff?

When I'm saying devleopers I mean anyone that codes, or is in need for running concealed and "clean" environments, or would like to test different environments on their own machine.

You may ask, why would anyone do that?

Well, the simple questions is, that containers are the answer to "It works on my machine", in that we can run the same code that "runs on my machine" but in a container, the same one that runs in production.

If the code passes tests and runs inside the container too, well, then it actually works. Anywhere.

But how do we get to this point of the code actually running in a container? 

Let's start from the beginning -> 

[Minimizing the speaker to the corener of the screen / switching with the actual terminal screen]

Let's say I'd like to run an nginx proxy server locally, maybe to test nginx with some different configurations or just make sure I deploy it correctly

I need to pull a container from the public docker store (what used to be called "hub):

`docker pull nginx`

I notice a few things, 
1. I'm pulling "latest" tag -> I did not set a tag like "v1/v2" like so: `nginx:1.13.12`
The different versions are available on store.docker.com.
2. I'm pulling a few different things which are called "layers". Layers are essentially image shards which represent logical parts of a docker images, that can be shared among different images and can also be reused when building the same images over and over.

Now, in order to run the already pulled container I can simple run 

`docker run nginx`
TBH, I could have just used `docker run nginx` to begin with and docker would have pulled my image from the remote repo.

Running the container I see nothing happens, but if we open a different shell [ open a tmux split ] we can see the container:
`docker ps`


