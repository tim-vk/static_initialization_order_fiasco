# Static initialization order fiasco (or is it?)

This week we have a little software mystery!

The keen eyed among you probably noticed I refactored `constants.configData.h`. I had to add some subfolders. Being the lazy programmer I am I used some variables:

```
		inline static const std::string vardir_ = "../var/"s;
		inline static const std::string logsdir_ = vardir_ + "log/"s;
		inline static const std::string piddir_ = vardir_ + "run/"s;
```
etc..

I ran it, tested it, it was reviewed & pushed to develop. Everything fine & dandy.

## The problem
So Kartik ran the code on his machine. And he got a bunch of errors! After some cursing and debugging the conclusion was: `logsdir_` only contains `"log/"`! `vardir_` is seen as empty!. 

## the (quick) solution
We were both (vaguely) aware of the [static initialization order fiasco](https://en.cppreference.com/w/cpp/language/siof) so we named that as the cause: `vardir_` had not yet been initialized when `logsdir_` was made & that was it. The (quick) solution was to just not use variables within `inline static const`'s. 

## But something was nagging in the back of my mind...

Why would this be an issue? The static initialization order fiasco is only an issue when different translation units are starting in different orders. A "translation unit" (as i'm aware of) of a `.cpp` file and it's included headers. Everything within the same header file should be within the same translation unit. So I wouldn't expect issues.

## The challenge

This is truely annoying me. So i've been trying if I could get a minimum working example of this issue. But to no avail.
The first person to modify [this repository on github](https://github.com/tim-vk/static_initialization_order_fiasco.git) in such a way that the same issue occurs gets a prize in the shape of **candy**. 

Note: the initialization order fiasco might not even be the cause! Might be something completely else. But i want to know. To win the prize you need to:
- Explain why the issue occured.
- Reproduce the issue in a minimal working example.