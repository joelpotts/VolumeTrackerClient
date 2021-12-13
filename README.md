# Volume Monitor

This is a tool to help monitor the volume levels of a location, either locally or remotely.

## What is the goal of this project?

I have recently struggled with some downstairs neighbours, who claim that we are being too noisy in our apartment. They claim that we play loud music, shout, stomp, and "bang on pipes" (despite there being no pipes in our apartment to bang on). It goes without saying that we do none of these things. I tried to find a solution to cheaply measure and monitor the volume inside our apartment, but could not find anything that suited my needs. What started out as the desire to measure the volume of our apartment, turned into a tool to also stream that data to anyone who wanted to know, downstairs neighbours for example. These are the main goals of the project:

- Be able to measure and graph the volume of a room via a microphone.
- Be able to broadcast that data so that it can be monitored remotely (i.e. an excuse to finally learn websockets and Socket.io).
- To be able to maintain a historical record of the volume data (TODO)
- To send an alert when the volume exceeds a certain threshold (TODO)

## Running the project

To run the project fully you will also need the [backend](https://github.com/joelpotts/VolumeTrackerServer)

### Running the backend

```bash
npm install
node index.js
```

### Running the frontend

```bash
npm install
npm run dev
```
