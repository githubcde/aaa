const express = require('express');
const mongoose = require('mongoose');
const bodyParser = require('body-parser');

const app = express();

// Mongoose 모델 정의
const UserSchema = new mongoose.Schema({
  username: String,
  email: String,
  // 다른 필요한 필드들
});

const User = mongoose.model('User', UserSchema);

// MongoDB 연결
mongoose.connect('mongodb://localhost:27017/mydatabase', {
  useNewUrlParser: true,
  useUnifiedTopology: true
})
.then(() => console.log('Connected to MongoDB'))
.catch(err => console.error('Could not connect to MongoDB', err));

app.use(bodyParser.json());

// 사용자의 프로필 정보를 가져오는 API endpoint
app.get('/api/user/:id', async (req, res) => {
  try {
    const user = await User.findById(req.params.id);
    if (!user) {
      return res.status(404).send('User not found');
    }
    res.send(user);
  } catch (error) {
    res.status(500).send('Server error');
  }
});

const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
