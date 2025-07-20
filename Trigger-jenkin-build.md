#✅ Step-by-Step: Trigger Jenkins Build on Git Push

#✅ 1. Install “GitHub Integration” Plugin
On Jenkins:

Go to Manage Jenkins → Manage Plugins

Under Available (or Installed tab if already there), make sure this is installed:

✅ GitHub Plugin

✅ GitHub Integration Plugin

✅ GitHub Branch Source (for multibranch pipelines)

Restart Jenkins after installation

#✅ 2. Configure GitHub Webhook
In your GitHub repo:

Go to Settings → Webhooks

Click Add Webhook
payload url: http://<your-ec2-public-ip>:8080/github-webhook/
Content type: application/json

Events: Choose Just the push event

Click Add Webhook

#✅ 3. Configure Jenkins Job for GitHub Hook Trigger
Open your Jenkins job → Configure

Under Build Triggers, check:

✅ GitHub hook trigger for GITScm polling

Under Source Code Management, make sure:

Git repo URL is correct (https://github.com/username/repo.git)

Correct branch (like main or develop)

Credentials added if private

