---
difficulty: Easy
status: Done
---
HERE’s a project that I’ve done earlier this year. With the introduction of Go as the new programming language on my company’s backend tech stack, the team needed a replacement for the language-specific RPC library; so I took the initiative to create one, using gRPC.

I went one step further, by trying to future-proof it: can my library support any language that gRPC supports? if the team ever decided to add Python or Rust microservice, will we still be able to use this library? “That would be neat”, I thought to myself. And here’s the result - you can check out the repository in the link below:

[https://github.com/adibfarrasy/grpc-polyglot-generator](https://github.com/adibfarrasy/grpc-polyglot-generator)

As of now, the repository already has Java, Kotlin and Go plugins to generate proto files and gRPC methods in those target languages. Adding support for another language is as easy as adding a gRPC plugin file for the new target language in the project root directory and update the generator script. For detailed instructions, read the project README (I tried to write the README as concise and comprehensive as possible, but if you have any question feel free to leave a comment!).

I also used Git submodules for importing the generated files (which is good in my context, since Go has first class support for importing libraries using just Git URLs). This part of the repository will be very snowflake-y, so you may want to use other methods of importing the generated files to your own projects.

The company ended up not using the library, as the team decided to go for a more battle-tested REST HTTP. A part of me still thinks it’s a shame I didn’t get to try out a newer technology in production haha. It feels like a waste for it to not be used, so I’d just share it. Hope it’s helpful to some of you 😃