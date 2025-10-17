# Discord Bot with URL Shortener

A Discord bot integrated with an Express.js server that provides URL shortening functionality. Users can interact with the bot to create shortened URLs that are stored in MongoDB.

## Features

- 🤖 **Discord Bot Integration** - Interactive bot that responds to user commands
- 🔗 **URL Shortening** - Create short URLs for long links
- 💾 **MongoDB Storage** - Persistent storage of URLs and their shortened versions
- 🚀 **Express Server** - RESTful API for URL management
- ✨ **Custom Short IDs** - Generates unique 6-character alphanumeric IDs

## Technologies Used

- **Node.js** - Runtime environment
- **Discord.js v14** - Discord bot framework
- **Express.js** - Web framework
- **MongoDB & Mongoose** - Database and ODM
- **Axios** - HTTP client for API requests
- **Nanoid** - Unique ID generator
- **dotenv** - Environment variable management

## Project Structure

```
discord-bot/
├── server.js                    # Express server setup
├── discordApp.js               # Discord bot configuration
├── command.js                  # Discord slash commands registration
├── connection.js               # MongoDB connection handler
├── package.json                # Project dependencies
├── controllers/
│   └── urls.controllers.js     # URL shortening logic
├── models/
│   └── urls.models.js          # URL MongoDB schema
└── routes/
    └── urls.routers.js         # API routes
```

## Prerequisites

Before running this project, make sure you have:

- Node.js (v16 or higher)
- MongoDB (local or cloud instance)
- Discord Bot Token ([Create a bot](https://discord.com/developers/applications))
- Discord Application Client ID

## Installation

1. **Clone the repository**

   ```bash
   git clone <repository-url>
   cd discord-bot
   ```

2. **Install dependencies**

   ```bash
   npm install
   ```

3. **Set up environment variables**

   Create a `.env` file in the root directory with the following variables:

   ```env
   TOKEN=your_discord_bot_token
   CLIENT_ID=your_discord_application_client_id
   MONGODB_URL=your_mongodb_connection_string
   PORT=3000
   EXPRESS_URL=http://localhost:3000
   ```

4. **Configure Discord Bot**
   - Go to [Discord Developer Portal](https://discord.com/developers/applications)
   - Create a new application
   - Go to the "Bot" section and create a bot
   - Copy the token and add it to your `.env` file
   - Enable "Message Content Intent" in the Bot settings
   - Go to OAuth2 → URL Generator
   - Select scopes: `bot`, `applications.commands`
   - Select bot permissions: `Send Messages`, `Read Messages/View Channels`
   - Use the generated URL to invite the bot to your server

## Usage

### Starting the Application

1. **Start the Express Server**

   ```bash
   node server.js
   ```

   The server will run on `http://localhost:3000`

2. **Start the Discord Bot**

   ```bash
   node discordApp.js
   ```

3. **(Optional) Register Slash Commands**
   ```bash
   node command.js
   ```

### Discord Bot Commands

#### 1. **Hello Command**

```
Hello
```

The bot will greet you with your Discord username.

#### 2. **Create Short URL**

```
create <URL>
```

**Example:**

```
create https://www.example.com/very/long/url/path
```

**Response:**

```
Shortened URL: http://localhost:3000/abc123
```

### API Endpoints

#### Create Short URL

- **Method:** POST
- **Endpoint:** `/create`
- **Body:**
  ```json
  {
  	"url": "https://www.example.com"
  }
  ```
- **Response:**
  ```
  http://localhost:3000/abc123
  ```

#### Redirect to Original URL

- **Method:** GET
- **Endpoint:** `/:shortId`
- **Example:** `http://localhost:3000/abc123`
- **Action:** Redirects to the original URL

## How It Works

1. User sends a message in Discord starting with `create` followed by a URL
2. The Discord bot validates the URL format
3. Bot sends a POST request to the Express server with the URL
4. Server generates a unique 6-character ID using Nanoid
5. URL and short link are stored in MongoDB
6. Server returns the shortened URL to the bot
7. Bot replies to the user with the shortened URL
8. When someone visits the short URL, they're redirected to the original URL

## Database Schema

### URL Model

```javascript
{
  url: String,        // Original URL
  shortId: String,    // 6-character unique ID
  shortLink: String,  // Full shortened URL
  timestamps: true    // createdAt, updatedAt
}
```

## Development

### Running in Development Mode

You can use `nodemon` for auto-reloading during development:

```bash
npm install -g nodemon
nodemon server.js
nodemon discordApp.js
```

## Error Handling

- Invalid URLs are rejected with a message
- Database errors are caught and logged
- Bot only responds to non-bot messages to prevent loops
- 404 responses for non-existent short links

## Future Enhancements

- [ ] Add slash commands for URL shortening
- [ ] Implement URL analytics (click tracking)
- [ ] Add URL expiration functionality
- [ ] Custom short IDs (user-defined aliases)
- [ ] URL validation and security checks
- [ ] Rate limiting
- [ ] User authentication and URL management
- [ ] Admin commands for bot management


## Support

If you encounter any issues or have questions, please open an issue on the repository.

---

**Note:** Make sure to keep your `.env` file secure and never commit it to version control. Add `.<env>` to your `.gitignore` file.
