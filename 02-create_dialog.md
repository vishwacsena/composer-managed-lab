# Add a dialog

When building features of a bot with Composer, it is sometimes useful to create a new `dialog` to contain a chunk of functionality. This helps keep the dialog system organized, and also allows sub-dialogs to be combined into larger, more complex dialogs. 

Remember - each dialog contains one or more triggers that launch associated actions. They can have their own dedicated language model. Dialogs can call other dialogs and can pass values back and forth.

![](./assets/01/adaptive-dialog-anatomy.png)

## What are we building?

The main feature of this bot is reporting on the current weather conditions.

To do this, we'll create a dialog that 
- prompts user to enter a zipcode (to use as location for weather lookup)
- calls an external API to retrieve the weather data for a specific zipcode.

First, we'll set up all the components and make sure they work together. Then, we'll flesh out the functionality.

## Create a new dialog

1. Click the `+ New Dialog` button in the left hand explorer. A dialog will appear and ask for a `name` and `description`
2. Give this new dialog the name:
    
      `getWeather`
    
    and the description:
    
      `Get the current weather conditions`

3. Click `Next`, and Composer will create the new dialog and open it in the editor.  

   ![](./assets/02/create-getweather.png)

Composer created this new dialog with a trigger already defined - `BeginDialog`. This trigger will is associated with a special event that fires immediately whenever a dialog begins. We can use this to make our dialog do stuff immediately, without waiting for further input.

4. Make sure to select `BeginDialog` in the left hand nav.

5. For now, we'll just add a simple message to get things hooked up, then we'll come back to flesh out the feature. Click the "+" in the flow, and use the same `Send a response` action.  Set the text of the activity to:
   
      `Let's check the weather`

   You'll have a flow that looks like this:

   ![](./assets/02/getweather-draft.png)

## Wiring up dialogs
You can break pieces of your conversation flow into `dialogs` and can chain them together. Let's get the newly created `getWeather` dialog wired up to the root dialog.

1. Click on `WeatherBot.Main` from the left navigation tree. After selecting `weatherBot.Main` from the explorer, find the `Language Understanding` section of the property editor. 

   > Each dialog can have it's own `recognizer`, a component that lets the bot examine an incoming message and decide what it means by choosing between a set of predefined `intents`. Different types of recognizers use different techniques to determine which intent, if any, to choose.

   > For now, we're going to use the `Regular Expression` recognizer, which uses pattern matching. Later, we'll use more sophisticated natural language understanding technology from `LUIS`.

2. Under the `Recognizer Type`, select `Regular Expression`

   ![](./assets/02/regexp-recognizer.gif)

3.  Click the `Add` button. 2 new fields will appear: one labeled `intent` and one labeled `pattern`

   ![](./assets/02/weather-intent.png)

4. Define the bot's first intent. Set the value of the `intent` field to:

      `weather`

5. Set the value of the `pattern` field to:

      `weather`

   > This tells the bot to look for the word "weather" anywhere in an incoming message. Regular expression patterns can be much more complicated than this, but for now, this will do!

6. Click "+ New Trigger" in the left hand side under the `weatherBot.Main` header, and a modal will appear. Select `handle an intent` from the first dropdown, and then select our freshly created `weather` intent from the second dropdown.

   ![](./assets/02/weather-trigger.gif)

7. Click `Submit` and a second trigger, along with _a new flow_ will appear in the main window.

8. Click the "+" in the flow and select the `Dialogs management > ` option. From the submenu, select `Begin a new dialog`

9. In the property editor for the new action, set the `dialog name` property to  our `getWeather` dialog.

## Create a Fallback 

Your bot can now handle requests for the weather. But what if a user sends an off topic message or requests something else? The bot should at least send a useful response. 

To do this, we can use an "Unknown Intent" trigger that will fire whenever a message isn't handled by another trigger.

1. Click on "+ New Trigger" in the left hand side under the `weatherBot.Main` header. The trigger modal will appear.  

2. Select `Handle Unknown Intent` from the first dropdown.  Click `Next`

3. You'll see a new flow in the main window. Click the "+" button, and select `Send a response`

4. Set the message text to:

      `What’s that? Right now, I only know how to check the weather.`

## Let's test it out.

1. Click the `Restart Bot` button in the upper right hand corner of the Composer window.  This will update the bot runtime app with all the new content and settings. Then, click `Test in Emulator`. When Emulator connects to your bot, it'll send the greeting we configured in the last section.

   ![](./assets/02/restart-bot.gif)

2. Send the bot a message that says `weather`. The bot should respond with our test message, confirming that our intent was recognized as expected, and the fulfillment action was triggered.

   ![](./assets/02/emulator-weather-draft.png)

3. Try something that the bot will not recognize - e.g. `Hello`. The bot should respond with the message for the `unknown intent` trigger.

## Covered in this section

- Created a sub-dialog that will contain our current weather feature
- Configured the regular expression recognizer to do regex-pattern-based recognition for an `intent` called `weather`
- Set up a trigger and handler for the `weather` intent
- Set up an action to fulfill the intent by starting the sub-dialog.