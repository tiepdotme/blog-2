---
layout: post
title: "Rest API using Deno and Oak"
image: "/assets/social/deno.png"
date: 2020-07-12 12:00:00 -0500
---

### What is Deno?

An anagram for Node and a JavaScript/Typescript runtime by Ryan Dahl (creator of Node). The runtime is trying to overcome the shortcomings of Node, but it is not the second coming of Node.

It has a ton of advantages: secure by default, supports TypeScript, ships as a single file (no dependencies) and import of ES modules from URL (like a browser would).

The runtime matured to version 1 in May of 2020. [Read the blog post from the creators](https://deno.land/v1) to understand where it is now, what they plan to do in the future and how they are planning to mitigate issues that Node brought with it.

### What am I doing?

Everyone is talking about the runtime, and I wanted to try it out. I wanted to see what the fuss is about, and I tried to subside my curiosity. I picked up the runtime today for the very first time. I created an endpoint (that implements the age-old Fizz Buzz challenge) and documented it. That is what I am going to share today!

### Problem statement

We will be building an endpoint that takes in a number as route parameter and returns status 200 with a message of either "Fizz", "Buzz" or "FizzBuzz". If you do not know the Fizz Buzz challenge, here is the problem statement.

```text
Create an endpoint that takes a number.
If the number is a multiple of 3, return "Fizz".
If the number is a multiple of 5, return "Buzz".
For number which is a multiple of both 3 and 5, return "FizzBuzz".
If it is not any of these cases, return the original number.

Assume that the input will always be a number.
```

### Source code

The full source code is available on my [GitHub](https://github.com/bhagatparwinder/deno-oak-fizz-buzz-api). If you can think of ways to make it better, shoot a PR. I am relatively new to this so I can use some feedback.

### Solution

We will first create `server.ts`. The file is the main entry point for our server. It is a typescript file that defines the port, event listener and imports routes (from our declared route map). I have divided the solution into three files: `server.ts`, `routes.ts` and `controller.ts`. You could have a ton of additional files in a real-world server.

#### Server

```typescript
import { Application } from "https://deno.land/x/oak/mod.ts";
import { bold, green } from "https://deno.land/std@0.53.0/fmt/colors.ts";
import router from "./routes.ts";
const port = 8000;
const app = new Application();

app.use(router.routes());
app.use(router.allowedMethods());

app.addEventListener("listen", ({ secure, hostname, port }) => {
  const protocol = secure ? "https://" : "http://";
  const url = `${protocol}${hostname ?? "localhost"}:${port}`;
  console.log(`${bold("server is listening on :")} ${green(url)}`);
});

await app.listen({ port });
```

To help us in setting up REST API endpoint quickly, I have utilized [Oak](https://deno.land/x/oak). If you are familiar with Express, you will feel right at home.

Oak initializes the application using `Application`. You can add routes to the application instance, attach middleware for authentication, version check and so on.

You can see how easy it was to import the dependency using the fully qualified URL. I did the same with the `colors` dependency. It is used to display the local server URL.

1. I have imported routes from a `routes.ts` file. We will go over it next.
2. I have also established the post as `8000`. You can, for the most part, use any port (besides reserved ports).
3. The `Application` class has two methods `use` and `listen`. `use` allows me to use the routes defined for the current instance of the application.
4. `allowMethods` tells Deno to allow all methods for the imported routes like `GET, POST, PUT, DELETE`. We only use GET in this application.
5. Once the server starts, before it starts processing requests, the application fires a "listen" event. We will be listening to it using the `addEventListener`. I use this to display a fully qualified URL in the console. This URL is where our Deno server is running.

### Routing

```typescript
import { Router } from "https://deno.land/x/oak/mod.ts";
import { helloWorld, calculateFizzBuzz } from "./controller.ts";

const router = new Router();

router
    .get("/", helloWorld)
    .get("/fizzbuzz/:id", calculateFizzBuzz);

export default router;
```

This file defines the routes of endpoints that are available in my application. I have imported `Router` from Oak and created a new instance of it as `router`.

There are also two methods that I have imported from `controller.ts`. One method (`helloWorld`) simply displays a message of "Hello World" and the other (`calculateFizzBuzz`) solves our problem statement above.

The instance of `Router` is chained to define two `GET` routes. You can mix route verbs. So if you have `POST, PUT` or `DELETE`, you can chain them with `GET`. It takes two parameters, a path and a function. The path is what the user will hit and the function is what will serve the logic for that path.

For our problem, I have used the path `/fizzbuzz`. You will see that I have suffixed the path with `/:id`. Express used the same technique to pass dynamic parameters. I can use this `id` param later to do operation on it. This `id` is how a user will give me the number they would like to analyze.

### Controller

```typescript
export const helloWorld = ({ response }: { response: any }) => {
  response.body = {
    message: "Hello World!",
  };
};

export const calculateFizzBuzz = ({ params, response }: { params: { id: string }, response: any }) => {
    let message = "";
    const input = parseInt(params.id);

    if(input % 15 === 0) {
        message = "FizzBuzz";
    } else if(input % 3 === 0) {
        message = "Fizz";
    } else if(input % 5 === 0) {
        message = "Buzz";
    } else {
        message = input.toString();
    }

    response.body = {
        message
    }
}
```

I have put both the functions, serving two routes in the same controller. It is okay for application as small as this one but maybe for an enterprise application you would want them separate.

The `helloWorld` function simply returns a response body with a hardcoded object containing the message "Hello World". Nothing to see there.

The `calculateFizzBuzz` also return a response body with an Object, but the message is dynamic in this case. We have access to the params Object which provides us with the id (or number) given by the user. We apply the logic to find if the number is divisible by 3, 5 or both and update the message accordingly. If it does not fall into any of those, I simply return the number.

### Running the application

You can clone my [GitHub repo](https://github.com/bhagatparwinder/deno-oak-fizz-buzz-api) or copy-paste data from this blog post. Change directory into the directory with the source code and execute.

```bash
deno run --allow-net server.ts
```

🚨 Make sure Deno is installed on your system before executing the above command. Here are the [installation instructions](https://github.com/denoland/deno_install).

Remember, I mentioned that Deno is secure by default? It does not allow any file, network, or environment access unless explicitly enabled. If you want your Deno application accessible on the port specified, you have to use the flag `--allow-net` between `run` and file name.

Once executed, you will see this on the terminal:

```bash
Check file:///Users/Parwinder/Workspace/deno-oak-fizz-buzz-api/server.ts
server is listening on : http://localhost:8000
```

You are ready to roll! Hit the endpoints to test the application. Use your favorite REST Client like Postman or execute a cURL command in the terminal.

### Testing endpoints

Going to `127.0.0.1:8000/` should give you a 200 status with the body:

```json
{
    "message": "Hello World!"
}
```

Going to `127.0.0.1:8000/fizzbuzz/9` should give you a 200 status with the body:

```json
{
    "message": "Fizz"
}
```

Going to `127.0.0.1:8000/fizzbuzz/50` should give you a 200 status with the body:

```json
{
    "message": "Buzz"
}
```

Going to `127.0.0.1:8000/fizzbuzz/90` should give you a 200 status with the body:

```json
{
    "message": "FizzBuzz"
}
```

I hope the article helps you to kickstart development with Deno, and as always, I am open for feedback. ❤️

Happy coding 👋🏼
