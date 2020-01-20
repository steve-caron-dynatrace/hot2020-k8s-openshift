# Exercise #9 Set up alert notifications

<u>Scenario:</u> The <b>guestbook</b> app is still very early in development. For now the team only want to receive alert notifications by email. 

On the other hand, the team in charge of support/ops of the Sock Shop frontend uses a chatops tool such as Slack. They want to receive alert notifications on a Slack channel, but only for alerts affecting the frontend services and running in production.

## Create an Alerting Profile for the guestbook app

- Make sure you are now logged in Dynatrace with your student account
- Go in <b>Settings -> Alerting -> Alerting profiles</b>
- Create a new profile named : `guestbook`
- Expand the <b>Custom alert rule</b>, select `"Only include entities that have any tags"`
- Click <b>Create tag filter</b> and select `[Kubernetes]product:guestbook` 
- Click <b>Save</b> then <b>Done</b>

![alerting_profile_guestbook](assets/alerting_profile_guestbook.png)

## Create an Alerting Profile for Sock Shop frontend production

- Create a new profile named : `sockshop frontend production`
- Expand the <b>Custom alert rule</b>, select `"Only include entities that have all tags"`
- Click <b>Create tag filter</b> and select `[Kubernetes]product:sockshop`
- Click <b>Create tag filter</b> and select `[Kubernetes]tier:frontend`
- Click <b>Create tag filter</b> and select `[Kubernetes]stage:prod`
- Click <b>Save</b> then <b>Done</b>

![alerting_profile_sockshop](assets/alerting_profile_sockshop.png)

## Setup Problem notification via email

You will set up problem notification to send emails on guestbook alerts.

- Go in <b>Settings -> Integrations -> Problem notifications</b> 
- Click on <b>Set up notifications</b> 
- Select <b>Email</b>
- Enter an email address
  - Use a real email address that you can access 
- At the bottom, select the `guestbook` Alerting Profile
- Click <b>Save</b>

## Setup Problem notification via Slack

You will set up problem notification to send messages to a Slack channel on Sock Shop production frontend-related alerts.

- Click on <b>Set up notifications</b> 
- Select <b>Slack</b>
- Enter a name of your choice for the notification rule
- Use the following webhook URL:
  - `https://hooks.slack.com/services/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`
- Enter the following channel: `#support-frontend`
- In title, enter `<your_name> {State} {ProblemTitle} {ImpactedEntity}`
  - Replace <your_name> with your name or whatever you want
- At the bottom, select the `sockshop frontend production` Alerting Profile.
- Click <b>Send test notification</b> to validate your configuration 
- Click <b>Save</b>.

![slack_integration](assets/slack_integration.png)

## Create custom alerts

In order to complete this exercise and have alerts being sent, we need to create some dummy alert conditions. We will use very low threshold to make sure an alert is always triggered.

### Create custom alert for guestbook

- Go in <b>Settings -> Anomaly Detection -> Custom events for alerting</b>.
- Click <b>Create custom event for alerting</b>
- Provide a title (of your choice) to the event rule
- In Category, select `Services`
- In Metric, select `Response time`
  
    ![custom_alert_guestbook](assets/custom_alert_guestbook.png)

- We need to filter this to guestbook services only
  - Click on <b>Add rule-based filter</b>
  - Select `Management zone` `equals` `app:guestbook`
  - Click <b>Create rule-based filter</b>

    ![custom_alert_filter_guestbook](assets/custom_alert_filter_guestbook.png)

  - The scope should only include the guestbook service

    ![custom_alert_scope_guestbook](assets/custom_alert_scope_guestbook.png)

- In the <b>Threshold</b> section, change the rule to:
  - Raise an alert if Response time is `above` the threshold value of `1` `MicroSecond (us)` for `1` minutes during any `3` minute period.

    ![custom_alert_threshold](assets/custom_alert_threshold.png)

- Click <b>Create custom event for alerting</b>
- <b><u>IMPORTANT</b></u> : Make sure you run a few transactions through the guestbook web app so Dynatrace can measure a Response Time.</b>

### Create custom alert for Sock Shop production frontend

- Create a new custom event for alerting as you did in the previous step
- We need to filter this to Sock Shop production and frontend only services
  - <u>Hint</u>: You will need to create 2 filters and use tags

    ![custom_alert_scope_sockshop_frontend_production](assets/custom_alert_scope_sockshop_frontend_production.png)
  
- Use same threshold as previous step

## Validate email notification

- Look at your Inbox for the email address you provided for the problem notification
- You should have received an email from "Dynatrace team" about an OPEN Problem

    ![email_notification](assets/email_notification.png)
  
## Validate Slack notification

- Access the Slack channel via this URL : https://hot-k8s.slack.com/archives/CSSET9TS9 
- User name : `perform@email.com`
- Password : `performHOT`
- Keep an eye on the `#support-frontend` channel for your alert notification!

    ![slack_notification](assets/slack_notification.png)

---

[Previous : #8 RBAC with Management Zones](../08_RBAC_with_Management_Zones) :arrow_backward: :arrow_forward: [Next : #10 Configure k8s cluster monitoring integration](../09_Set_up_alert_notifications)

:arrow_up_small: [Back to overview](../)