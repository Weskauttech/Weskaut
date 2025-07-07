const express = require('express');
const mongoose = require('mongoose');
const cors = require('cors');
const playerRoutes = require('./routes/playerRoutes');

const app = express();
app.use(cors());
app.use(express.json());
app.use('/api/players', playerRoutes);

mongoose.connect('mongodb://localhost:27017/weskautDB')
  .then(() => console.log('MongoDB connected'))
  .catch(err => console.error(err));

app.listen(5000, () => console.log('Server running on port 5000'));
// /client/src/components/PlayerList.js
import React, { useEffect, useState } from 'react';
import axios from 'axios';

function PlayerList() {
  const [players, setPlayers] = useState([]);

  useEffect(() => {
    axios.get('http://localhost:5000/api/players')
      .then(res => setPlayers(res.data));
  }, []);

  return (
    <div className="p-6">
      <h1 className="text-2xl font-bold mb-4">Football Players</h1>
      <ul>
        {players.map(player => (
          <li key={player._id} className="mb-2">
            <strong>{player.name}</strong> - {player.position} ({player.team})
          </li>
        ))}
      </ul>
    </div>
  );
}

export default PlayerList;
import cv2
import mediapipe as mp

def analyze_video(video_path):
    cap = cv2.VideoCapture(video_path)
    mp_pose = mp.solutions.pose
    pose = mp_pose.Pose()

    while cap.isOpened():
        success, frame = cap.read()
        if not success:
            break

        results = pose.process(cv2.cvtColor(frame, cv2.COLOR_BGR2RGB))

        if results.pose_landmarks:
            for lm in results.pose_landmarks.landmark:
                print(f'Landmark: {lm.x}, {lm.y}, {lm.visibility}')
        
    cap.release()

if __name__ == "__main__":
    analyze_video('sample_match.mp4')
npm install multer
const multer = require('multer');
const upload = multer({ dest: 'uploads/' });

app.post('/api/upload', upload.single('video'), (req, res) => {
  res.json({ path: req.file.path });
});
