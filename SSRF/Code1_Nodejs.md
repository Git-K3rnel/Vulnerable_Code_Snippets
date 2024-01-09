```javascript
const express = require('express');
const fetch = require('node-fetch');

const app = express();

app.get('profile-picture', assync (req, res) => {
  const imgURL = req.query.imgurl;

  // if no parameter is supplied, return an error
  if (!imgURL?.length) return res.status(500).json({error: 'Internal Server Error'});

  try {
    // retrieve the image
    const response = await fetch(imgURL);

    // check if the image exists
    if (response.ok) {
        // read response buffer
        const data = await response.buffer();

        // set content-type
        res.contentType('image/jpeg');

        // return response to client
        res.end(data);
    } else {

          // handle the error if the request was not successful
          const errorData = await response.json();
          res.status(response.status).json({error: errorData});
      }
  } catch (error){

    // handle any error that occured during the fetch
    console.error('error during fetch:', error)
    res.status(500).json({error: 'Internal server error'});
    }
});

app.listen(8000, () => {
  console.log('server is running on http://localhost:8000');
});
```
