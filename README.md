# tech-blog

Fullstack blog app where users can sign in, create posts, edit posts, comment on posts, and delete posts.

![screenshot](screenshot.png)

|                                         |                                         |                                                   |
| :-------------------------------------: | :-------------------------------------: | :-----------------------------------------------: |
|       [Introduction](#tech-blog)        | [Table of Contents](#table-of-contents) | [Development Highlights](#development-highlights) |
|      [Installation](#installation)      |             [Tests](#tests)             |       [Code Hightlights](#code-highlights)        |
| [Technologies Used](#Technologies-Used) |           [Credits](#Credits)           |                [License](#License)                |

## Development Highlight

- Incorporate Handlebars.js as a template engine for the front end.
- Implment built-in and custom helpers in Handlebars.js
- Define relations in database between models
- Create middleware functions for login.

## Installation

1. Create .env file with this content to configure MySQL to your computer:

```
DB_NAME=ecommerce_db
DB_USER=<YOUR_MYSQL_USERNAME>
DB_PW=<YOUR_MYSQL_USERNAME>
```

2. Install node modules

```
npm i
```

3. Source Schema

- Login to MySQL shell: `mysql -u <username> -p
- Run `source db/schema.sql

4. Start app.

```
npm start
```

Makes request using Port 3003.

## Tests

```
npm test
```

## Code Highlights

Create post route for users to login. A query is made to find the email. There is a validator to check if that user exists. Next, a validate is used to check the password. A session is used to let the user stay logged in.

```JavaScript
router.post('/login', (req, res) => {
    User.findOne({
        where: {
            username: req.body.username
        }
    }).then(dbUserData => {
        if (!dbUserData) {
            res.status(400).json({ message: 'No user with that username address.' });
            return;
        }
        const validPassword = dbUserData.checkPassword(req.body.password);

        if (!validPassword) {
            res.status(400).json({ message: 'Incorrect password!' });
            return;
        }
        req.session.save(() => {
            req.session.user_id = dbUserData.id;
            req.session.username = dbUserData.username;
            req.session.loggedIn = true;

            res.json({ user: dbUserData, message: 'You are now logged in!' });
        });
    });
});
```

Create a partials for handlebars using `this` and looping through it to provide the comment information.

```handlebars
<div class="comments">
    {{#each this}}
    <section class="comment">
        <div class="meta">
            {{user.username}} on {{format_date created_at}}
        </div>
        <div class="text">
            {{comment_text}}
        </div>
    </section>
    {{/each}}
</div>
```

## Technologies

### Backend Language

- [Node.js](https://nodejs.org/en/)

### Backend Framework

- [Express.js](https://expressjs.com/)

### Database

- [MySQL](https://www.mysql.com/)

### ORM

- [Sequelize](https://sequelize.org/)

### Template Engine

- [handlebarsjs](https://handlebarsjs.com/)

### User Accounts Dependencies

- [connect-session-sequelize](https://www.npmjs.com/package/connect-session-sequelize)
- [bctrypt](https://www.npmjs.com/package/bcrypt)

## Credits

|                           |                                                                                                                                                                                                       |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **David Anusontarangkul** | [![Linkedin](https://i.stack.imgur.com/gVE0j.png) LinkedIn](https://www.linkedin.com/in/anusontarangkul/) [![GitHub](https://i.stack.imgur.com/tskMh.png) GitHub](https://github.com/anusontarangkul) |

## License

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
