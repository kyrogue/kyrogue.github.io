---
title: Jenkins Office 365 Connector Plugin
date: 2020-10-04 21:16:25
tags: ["jenkins"]
categories:
- ["Learning"]
---
Moving from slack notifications to Microsoft Teams notifications for Jenkins, there was a few choices for the available plugin that can be used within Jenkins.
Generally, when looking at a Jenkins plugin to use, I would look at the health of the plugin in terms of whether it is actively maintained.
<!-- more -->
Eventually, settling on [office-365-connector-plugin](https://github.com/jenkinsci/office-365-connector-plugin).
If we look at how the plugin is integrated with Microsoft Teams, it is via the use of microsoft [incoming webhooks](https://docs.microsoft.com/en-us/microsoftteams/platform/webhooks-and-connectors/how-to/add-incoming-webhook) . This is a very simple and easy to get going integration, however it is not very secure in terms of confidentiality of the webhook URL, as anyone with the webhook URL could potentially post a webhook to your Microsoft teams channel.

If we look at the example given in the [github page](https://github.com/jenkinsci/office-365-connector-plugin#pipeline-step) , we can see that the example exposes the webhook URL when using it.
A better way would be to make use of Jenkins pipeline environment variables and using a text credential to store the webhook url.

Globally scoped environment, whereby the environment block is defined outside the `stages` block:
{% code Jenkinsfile %}
pipeline {
    ...
    environment {
        WEBHOOK_URL     = credentials('jenkins-text-id')
    }
    stages {
        stage('Example stage 1') {
            steps {
              office365ConnectorSend webhookUrl: "$WEBHOOK_URL",message: 'Application has been [deployed](https://uat.green.biz)',status: 'Success'
            }
        }
    }
}
{% endcode %}

Stage scoped environment, whereby the environment block is defined within the `stage` block:
{% code Jenkinsfile %}
pipeline {
    ...
    stages {
        stage('Example stage 1') {
          environment {
              WEBHOOK_URL     = credentials('jenkins-text-id')
          }
          steps {
            office365ConnectorSend webhookUrl: "$WEBHOOK_URL",message: 'Application has been [deployed](https://uat.green.biz)',status: 'Success'
          }
        }
    }
}
{% endcode %}
