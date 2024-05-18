# Log in Discord

Want something simpler than Sentry, Datadog, etc.? Do you want to log your app's information to Discord like a console.log in production? Simple and easy, with no cost? This is a good alternative.

This package allows you to easily log information to Discord using webhooks. You can specify different webhooks for different types of logs, and you can log any type of information.

See the example image below:

<img src="https://res.cloudinary.com/dr4xmxoal/image/upload/v1716004742/send-discord-log-lib/log-examples.png" alt="How your logs will look like in Discord" style="height: 350px; width: auto;">

I hope you find it useful. If so, buy me a coffee to keep me awake and coding!

<a href="https://www.buymeacoffee.com/fabiobcsouza">
<img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="width: 150px; height: auto;">
</a>

## Installation

Install the package using npm:

```sh
npm install discord-logger
```

## Configuration

Go to your Discord server, right-click on the channel you want to log to, and click on "Edit Channel". Go to the "Integrations" tab and click on "Create Webhook". Copy the webhook URL. You can create multiple webhooks for different types of logs.

Create a discord-logger.json file in the root of your project with the following structure:

```json
{
  "commands": [
    {
      "name": "default",
      "description": "Default logger, if you don't need to specify a label to the logger",
      "webhookUrl": "https://discord.com/api/webhooks/..."
    },
    {
      "name": "UserFlow",
      "description": "Log the user auth flow",
      "webhookUrl": "https://discord.com/api/webhooks/other-or-same..."
    }
  ]
}
```

You can create as many commands as you need. The name is the label you will use to log information. The webhookUrl is the URL you copied from Discord.

## Usage

Import the logger in your code and use it to log information to Discord:

```typescript
import { DiscordLogger } from "discord-logger";

// Example usage
const fakeData = { user: "John Doe", action: "login" };

// logs to default webhook
DiscordLogger.send({ label: "default", value: fakeData, type: "info" });

// logs to a custom webhook
DiscordLogger.send({
  label: "UserFlow",
  value: { user: "John Doe", step: "authenticated" },
  type: "success",
}); // logs to UserFlow webhook

try {
  throw new Error("This is an error");
} catch (error) {
  DiscordLogger.send({
    label: "default",
    value: error,
    type: "error",
  });
}
```

## Options

Method `send` has the following parameters:

| key   | type     | description                                                                |
| :---- | :------- | :------------------------------------------------------------------------- |
| label | `string` | **Required**. The label you used in the configuration file.                |
| value | `any`    | **Required**. The information you want to log.                             |
| type  | `string` | Optional. The type of log. Can be "info", "success", "warning" or "error". |

## Be aware

This package makes http requests to Discord's API. Consider the time it takes to log information to Discord, errors, etc.

This package does not have any kind of rate limiting but Discord does, if you log too much information, it can be ignored by for a while. If the log content has more than 2000 characters, it will be split into multiple messages called "chunks". So, if you have any doubts how Discord handles webhooks, check the [Discord documentation](https://discord.com/developers/docs/resources/webhook).

This package is not meant to replace Sentry, Datadog, etc. It is a simple way to log information to Discord. It is not meant to log sensitive information, as the information is sent to Discord in plain text.

## Support

Let me know that you like this package by buying me a coffee!

<a href="https://www.buymeacoffee.com/fabiobcsouza">
<img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="width: 150px; height: auto;">
</a>

## Contributing

Let me know if you have any ideas to improve this package.

## Issues

If you find any issues, please open an issue on the GitHub repository. I will be happy to help you. If you can, please provide a code snippet that reproduces the issue. It will help me to solve it faster.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
