# IC2S2
## _Easy, Unified Deployment_

What year is it? Push your website to that branch of this repo, and it will automatically go live at YEAR.ic2s2.org 

For example, for year 2025
```sh
git push -u origin 2025
```
will push it to the branch named "2025" 
This triggers Netlify to build and deploy that code, which is then reachable at 2025.ic2s2.org
You can work off the **2023** branch as a template (https://github.com/iscss/ic2s2/tree/2023)


## Netlify
There is a netlify account associated with this repo and maintained by ISCSS. At Netlify, select "Login with Github" and enter the credentials used to sign into this repo. 
(https://app.netlify.com/teams/iscss/sites)
## FAQ
- Why all the funny business with netlify and html forwarding? Why not just set up dns forwarding for all the year subdomains?
Well, this was tried, and for whatever reason several of the past websites would time out when we tried to use dns forwarding. Several dns providers were tried and it just didn't work. Additionally, more people are familiar with working with git than with dns providers, and simplifying things to merely pushing things to a repo is cleaner and less likely to cause issues in the future.
- What's up with all the branches in the repo? Why have a branch for each year?
Netlify has what's called "branch depoys." So each year has a branch, and that year's website is accessible at that year's subdomain, eg 2023.ic2s2.org will serve that years website, and pushing changes to the 2023 branch will automatically deploy those changes. Most of these branches are just links to external sites at the moment, but moving forward all future sites' content will be together in this one repo, with one hosting solution (Netlify.) Already previous year conference sites have gone down, and we can only reference archived versions that are a bit janky. By keeping things together in a unified format we can ensure the great work of these conferences remains available. 


