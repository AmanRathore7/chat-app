

## Run the backend app
$ npm run start:dev
Default port is 3000

## BACKEND INSTALLATION AND IMPLEMENTATION

Step 1:

```bash
$ npm install
```

Step 2:
#Install nest cli:
```bash
$ npm i -g @nestjs/cli
```

Step 3:
#Create nest project
```bash
$ nest new project-name
```

Step 4:
#Install socket package
```bash
$ npm i --save @nestjs/websockets @nestjs/platform-socket.io
```
```bash
$ npm i --save socket.io
```

Step 5:
#Generate service gateway and module gateway
```bash
nest generate service  gateway --no-spec
```

```bash
nest generate module gateway
```

Step 6:
#Enable CORS in WebSocketGateway (goto gateway.service.ts)

Step 7:
#Create connection with WebSocketServer and handle it by SubscribeMessage 
#Ensure that excepting self client id while emitting message

Step 8:
#Enabling Cache
```bash
npm install --save @nestjs/websockets cache-manager cache-manager-redis-store
```

Step 9:
NestJS application's main module (app.module.ts), configure Redis as a caching provider.

import { Module, CacheModule } from '@nestjs/common';
import { CacheInterceptor } from '@nestjs/common';
import * as redisStore from 'cache-manager-redis-store';

@Module({
  imports: [
    CacheModule.registerAsync({
      useFactory: () => ({
        store: redisStore,
        host: 'localhost',
        port: 3000, // Default Redis port
      }),
    }),
  ],
})

Step 10: gateway.service.ts
Inject the cache service into your Socket Gateway class and use it to cache data as needed.

import { Cache } from 'cache-manager';

Create constructor:
constructor(private cacheManager: Cache) {}

 async handleConnection(client: any) {
    const userId = this.getUserIdFromClient(client); // Example method to get user ID
    await this.cacheManager.set(`user:${userId}`, client.id);
  }

  async handleDisconnect(client: any) {
    const userId = this.getUserIdFromClient(client); // Example method to get user ID
    await this.cacheManager.del(`user:${userId}`);
  }


## Run frontend

Install server : npm install -g http-server
Run Command: http-server -p 4000


## Implementation the frontend app

Step 1:
# Add CDN of socket.io into Head

Step 2:
# Create connection to socket
const socketIo = io("http://localhost:3000");

Step 3:
# Get message from server via socket and display it to chat box
socketIo.on("message", function(data) {
    insertChat("you", data, 1000);
});

# Send message 
socketIo.emit("message", text);


