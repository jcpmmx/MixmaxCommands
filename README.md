# Mixmax Slash commands
> Originally developed as a take-home test project for Mixmax.

This test project enables Mixmax users to add support for a new Mixmax Slash command called `flight` that returns the
basic data of an upcoming flight, given by its flight code (e.g. `AA123`), using data from FlightAware API.

### How to run this on production
1. Open up the Mixmax Developer Dashboard and click `Add Slash Command`.
2. Enter the following inputs:
    - Name: `Get basic data and track a given flight`
    - Command: `flight`
    - Parameter placeholder: `[Flight code (e.g. AA123)]`
    - Command Parameter Suggestions API URL:  
`https://mixmax-jcpmmx.herokuapp.com/v1/commands/flight/suggestions/`
    - Command Parameter Resolver API URL:  
`https://mixmax-jcpmmx.herokuapp.com/v1/commands/flight/resolver/`
3. Refresh Gmail with Mixmax installed. Click `Compose` and type `/flight` to use this new command

### How to run this locally
1. Git clone `https://github.com/jcpmmx/MixmaxSC`
2. Make sure this Django project is set up and then run the development server: `python manage.py runsslserver`
3. Restart Chrome in a special temporary mode so the self-signed HTTPS urls can be loaded. See [here](https://developer.mixmax.com/docs/integration-api-appendix#local-development-error-neterr_insecure_response).
4. Verify it works by visiting `https://localhost:8000/v1/commands/flight/suggestions/?text=sia1` and `https://localhost:8000/v1/commands/flight/resolver/?text=sia1` in your browser. Both should return results.
5. Open up the Mixmax Developer Dashboard and click `Add Slash Command`.
6. Enter the following inputs:
    - Name: `Get basic data and track a given flight`
    - Command: `testflight`
    - Parameter placeholder: `[Flight code (e.g. AA123)]`
    - Command Parameter Suggestions API URL:  
`https://localhost:8000/v1/commands/flight/suggestions/`
    - Command Parameter Resolver API URL:  
`https://localhost:8000/v1/commands/flight/resolver/`
7. Refresh Gmail with Mixmax installed. Click `Compose` and type `/testflight` to use this new command

---

### About this solution
- App has been built using Python 2.7, Django 1.11 with Django REST Framework 3.7
- FlightAware API free tier is limited, both in terms of data (I can't show more useful data like airline name or airport terminal/gate) and throughput (like 500 queries/month, 5 per minute)
- I've added some simple DB cache to not query FlightAware API again after one suggestion gets picked
- Frontend code for the resolved widget can be improved; I just borrowed most styles from the Wiki Slash command
- This Django project includes some settings that are **definitely not recommended** for production environments, like having `DEBUG=True`, API keys in files (we could use `git-crypt`, for instance) and using DB for cache (we could use Redis instead). I chose them deliberately for the sake of simplicity.

### Nice to haves
- Add test cases
- Add more docstrings
- Use serilizers to manage data input/output
- Once we have serializers, add API docs
- Build a Node.js version of this command

---

### Feedback
- The resolver of a Slash command should be able to receive an ID from the selected suggestion instead of just the text
- When trying to create a new slash command with empty data, the message prompts says 'Command parameter hints
required': it should say whether 'Command parameter suggestion API URL' or 'Command parameter resolver API URL' (note
the casing I suggest). It's not clear whether all parameters are required or not.)
- The Developer settings should allow user-created slash commands to be editable, or at least to see its parameters for
reference
- API docs related to Slash commands don't explicitly say requests will be done using GET
- For consistency, you should refer to 'suggestions' rather than 'typeahead' when mentioning the Slash commands API
reference (e.g. like the `Tutorial` part of the Slash command API docs or the placeholder when adding a new command)
