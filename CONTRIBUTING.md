# How to Contribute

### Feature Request
You can request a new feature by opening an issue [here](https://github.com/nojam-org/nojam/issues/new?template=feature_request.yml). Please, make sure that you follow the template for new features and all sections have been completed.

### Implementing Features
To implement a feature, most of the time, you will want to create a new branch from the `main` one. Please give your branch a self-explanatory name.
```
git switch -C feature/{YOUR_FEATURE_NAME}
```
Once you are done with your local edits (and everything is committed), you can publish your branch for others to see.
```
git push --set-upstream origin feature/{YOUR_FEATURE_NAME}
```
Then, if a Pull Request is required for such a branch, go on [GitHub](https://github.com/nojam-org/nojam) and open the Pull Request from the website. Please, make sure you follow the template for new Pull Requests and all sections have been completed.

### Deliverable 1 (d1)
In `d1/` you can find a list of Markdown files, one per theoretical "Section" of the final d1 pdf. Each file contains a comment explaining what that file is about and another comment with a template to add an entry which should be copy-pasted. Please, keep the new entries sequential and avoid flooding the Pull Request with commits. Keep the commit relative with the session and add a clear and concise message. 
```
git commit -m "added RFs from 020 to 023"
```

**NOTE**: Edits in the entries should **NEVER** edit other entries tags nor delete them. 
