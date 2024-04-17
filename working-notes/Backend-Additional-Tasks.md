# Backend additional tasks: Expense tracker

- There is already text for assignment of Expense Tracker, with all the requirements and responsibilities.
        - This is more additional functional requirements for the Expense Tracker, like:
          - deployment
          - monitoring
          - security
          - performance
          - scalability
          - reliability
          - maintainability
          - testing
- This is a good example of how to add additional tasks to an existing assignment.

## Deployment
- The application should be containerized using Docker for ease of deployment and ensuring consistency across different environments.
- The application should be deployed on a cloud platform like AWS, Google Cloud, or Azure. Knowledge of cloud services like EC2, S3, Cloud Functions, etc., is required.
- Implement zero-downtime deployment strategies such as blue-green or canary deployments to ensure that the application is always available to users even during deployment.
- Configure environment variables and secrets securely for different environments (development, staging, production).
- Document the deployment process thoroughly so that any new team member can understand and follow it.

## Monitoring
- Implement a monitoring system using tools like Prometheus, Grafana, or New Relic to track the application's performance and health in real time.
- Set up alerts to notify the team of any significant changes in the application's performance or when the system encounters errors.
- Monitor the application's resource usage (CPU, memory, disk I/O, network) to identify potential bottlenecks and optimize accordingly.
- Track key performance indicators (KPIs) like response time, error rates, and request rates to ensure the application is performing as expected.
- Use log management tools like ELK Stack (Elasticsearch, Logstash, Kibana) or Splunk for centralized logging and log analysis.
- Document the monitoring strategies and tools used so that any new team member can understand and follow it.

## GDPR Compliance: Data Privacy and Retention
- Implement data encryption in db for sensitive user data to ensure that it is protected from unauthorized access.
- Implement data decryption only at the point of use. The application should decrypt data only when it is necessary for processing, and re-encrypt it as soon as possible afterwards.
- Implement a data retention policy that aligns with GDPR requirements. This includes automatically deleting user data after a certain period of inactivity, and providing users with the ability to request their data be deleted at any time.
- Ensure that the application supports the right to be forgotten. This means that when a user requests their data to be deleted, the application should be able to completely remove all of their personal data from the system.
- Audit and monitor access to sensitive data to detect any unauthorized access or anomalies.

## Notification System
- Implement a notification system to keep users informed about important events and updates related to their expenses.
- Publish notifications for events like expense approval, rejection, reminders for pending expenses, etc.
- Send different types of notifications (email, SMS, in-app) based on user preferences and the urgency of the event.
- Allow users to configure their notification preferences and frequency to avoid overwhelming them with unnecessary notifications.

## Contracts 
## 