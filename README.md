# Building and Deploying a Guestbook Application

This project focuses on building and deploying a guestbook application using Kubernetes and OpenShift. The application consists of a web front end allowing users to submit text via a text input field. Key tasks include Horizontal Pod Scaling, Rolling Updates and Rollbacks, utilization of OpenShift image streams, and deployment of a multi-tier version of the guestbook application.

## Tasks Overview:

- Modify the Dockerfile to accommodate the guestbook application's requirements.
- Ensure the guestbook image is correctly pushed to the IBM Cloud Container Registry.
- Verify the index page functionality of the deployed Guestbook – v1 application.
- Implement Horizontal Pod Autoscaler to manage pod scaling based on resource usage.
- Ensure the Horizontal Pod Autoscaler scales replicas correctly to handle varying loads.
- Execute Docker build and push commands to update the guestbook application.
- Configure deployment settings to enable autoscaling based on defined metrics.
- Verify the functionality of the updated index page for the deployed Guestbook – v2 application post-deployment rollout.
- Review the revision history for the deployment post-rollout to track changes and updates.
- Confirm the status of the updated deployment following a rollback to restore the previous version.
- Document the project with screenshots and detailed notes for reference in the `myNotes` document.
- Utilize OpenShift image streams to facilitate smooth rollout of updates.
- Deploy a multi-tier version of the guestbook application for enhanced functionality.

By completing these tasks, the project aims to demonstrate proficiency in building, deploying, and managing applications on Kubernetes and OpenShift platforms, ensuring efficient scalability, updates, and rollbacks as needed.
