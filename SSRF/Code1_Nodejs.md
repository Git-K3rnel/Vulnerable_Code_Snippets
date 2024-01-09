```javascript
const express = require('express');
const fetch = require('node-fetch');

const app = express();

app.get('profile-picture', assync (req, res) => {
  const imgURL = req.query.imgurl;

  if (!imgURL?.length) return res.status(500).json({error: 'Internal Server Error'});

  try {
    const response = await fetch(imgURL);

    if (response.ok) {
        const data = await response.buffer();

        res.contentType('image/jpeg');

        res.end(data);
    } else {
          const errorData = await response.json();
          res.status(response.status).json({error: errorData});
      }
  } catch (error){

    console.error('error during fetch:', error)
    res.status(500).json({error: 'Internal server error'});
    }
});

app.listen(8000, () => {
  console.log('server is running on http://localhost:8000');
});
```
