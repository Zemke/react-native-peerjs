# React Native PeerJS

[react-native-webrtc](https://github.com/react-native-webrtc/react-native-webrtc) has brought WebRTC to React Native. [PeerJS](https://github.com/peers/peerjs) is a simple API to work with WebRTC in the Browser.

I made it so that PeerJS works with react-native-webrtc in a React Native application.

## Install

Install react-native-webrtc according to their install guide. Then add react-native-peerjs:

```
yarn add react-native-peerjs
```

## Usage

Just refer to the PeerJS documentation. Here is an example:

```js
import Peer from 'react-native-peerjs';

const localPeer = new Peer();
localPeer.on('error', console.log);

localPeer.on('open', localPeerId => {
  console.log('Local peer open with ID', localPeerId);

  const remotePeer = new Peer();
  remotePeer.on('error', console.log);
  remotePeer.on('open', remotePeerId => {
    console.log('Remote peer open with ID', remotePeerId);

    const conn = remotePeer.connect(localPeerId);
    conn.on('error', console.log);
    conn.on('open', () => {
      console.log('Remote peer has opened connection.');
      console.log('conn', conn);
      conn.on('data', data => console.log('Received from local peer', data));
      console.log('Remote peer sending data.');
      conn.send('Hello, this is the REMOTE peer!');
    });
  });
});

localPeer.on('connection', conn => {
  console.log('Local peer has received connection.');
  conn.on('error', console.log);
  conn.on('open', () => {
    console.log('Local peer has opened connection.');
    console.log('conn', conn);
    conn.on('data', data => console.log('Received from remote peer', data));
    console.log('Local peer sending data.');
    conn.send('Hello, this is the LOCAL peer!');
  });
});
```

