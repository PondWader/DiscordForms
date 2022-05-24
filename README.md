# DiscordForms
 A library for creating forms for a discord.js bot

# Normal Usage Example

Here is an example of using DiscordForms to create a simple application form:
```
// Importing dependancies

const DiscordForm = require('discordforms');
const Discord = require('discord.js');

// Creating the form with a configuration
// None of the config fields are requires, but all of them are used here as an example

const form = new DiscordForm({
    timeout: 60000, // The time out time in miliseconds, 60000 = 60 seconds (this is how long the users will have to answer each question),
    failTries: 2, // How many tries the user gets to fail before the form is cancelled (0 means if the first answer is rejected the form is cancelled),
    pings: true, // Whether or not to highlight the form messages to the user by pinging them in plain text with the embed
    cancelPhrase: 'cancel', // The phrase that if a user says, cancels the form
    confirmSubmission: true // Whether or not the user will have to confirm that they want to submit the form
});

// Adding questions to the form

// This question will give the user the ability to react with numbers 1-3 choosing the role they are applying for
form.addQuestion('What role are you applying for?', 'multipleChoice', false, ['Helper', 'Partner Manager', 'Manager']);
// This question uses a custom filter to make sure the age entered fits the requirements
form.addQuestion('How old are you?', 'text', (answer) => {
    const number = Number(answer.content);
    if (!number) {
        return 'The age you provided is not a valid number!';
    } else if (number < 13) {
        return 'You must be 13 or older to apply as per Discord TOS!';
    } else return true;
});
// This question will give them a check mark or a cross to react with which they can answer the question with
form.addQuestion('Have you read the rules?', 'boolean');
// This question will expect a basic text answer
form.addQuestion(`Why do you think you'd be a good addition to our staff team?`, 'text');

// Setting up the bot

const bot = new Discord.Client();

bot.on('ready', () => {
    console.log('Logged in');
})

// Basic command listener for demo purposes

bot.on('message', msg => {
    // if the message content is "!apply" the form will be sent to the users DM's, the first msg.author is for the channel to send the form in, the 2nd msg.author is to state that they are the one completing the form
    if (msg.content.toLowerCase() === '!apply') {
        response = form.send(msg.author, msg.author).then(response => {
            // The code here will be triggered if the user succesfully completes the form
        }).catch(e => {
            // A promise rejection error will be caught here if the user fails to complete the form
        });
    }
})

// Logging in to the bot

bot.login('token');

```

# Step by step guide
### Creating a new form

The first thing you need to do to create a new form is import DiscordForms, you can do this as shown below (make sure you have DiscordForms installed):
```
const DiscordForm = require('discordforms');
```
Then you can add this to your code to create a new form:
```
const DiscordForm = require('discordforms');
const form = new DiscordForm(new DiscordForm({
    timeout: 60000, // The time out time in miliseconds, 60000 = 60 seconds (this is how long the users will have to answer each question),
    failTries: 2, // How many tries the user gets to fail before the form is cancelled (0 means if the first answer is rejected the form is cancelled),
    pings: true, // Whether or not to highlight the form messages to the user by pinging them in plain text with the embed
    cancelPhrase: 'cancel', // The phrase that if a user says, cancels the form
    confirmSubmission: true // Whether or not the user will have to confirm that they want to submit the form
}););
```
As you can see above you can enter in many options to customise the form. None of these options are required as they will have default values.

### Adding a question to the form

The function to add a question is: <DiscordForm>.addQuestion(question, type, filter, choices, subform );




# Applying custom embed styling

DiscordForms allows you to fully customise the embeds that are used and comes with a placeholder system to make this even easier!
The following code is an example of applying custom styling by creating a module in a new file which exports a function to set the embed styles:
```
```

# Using Filters

DiscordForms gives the capability to run filters on answers, these filters allow you to apply your own custom rules to answers that users submit which must be met.
Here is an example of using a custom filter in a question to make sure that the answer is a valid text channel:
```
form.addQuestion('Please enter the channel that you want welcome messages sent in.', 'text', (answer) => {
    if (!answer.mentions.channels.first()) {
        return 'You have not provided a valid channel!';
    } else return true;
});
```
If a custom filter returns a string, the answer will be counted as invalid and the string will be used as the error message.
If an answer meets the requirements of your filter however, you should just return the boolean value true.
If the question type is text, the Message class from discord.js will be fed in to the filter.
If the question type is boolean, true or false will be fed in to the filter, not a Message.
If the question type is multiple choice, the choice that the user picked will be fed in to the filter as a string.

# Handling Responses

# Docs 

https://github.com/PondWader/DiscordForms/link-finish
