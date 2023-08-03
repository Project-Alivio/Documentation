# Overview

This section provides an overview of Alivio's stack: the different programming languages, libraries, frameworks, and cloud services that we use to build the app. When designing a new feature, we require that you stay consistent with the current stack that we have and document any new additions if needed.

# Front End

- **Programming Language**: JavaScript, HTML, CSS 
- **Frameworks**: React.js 
- **Third Party React Libraries**
    - Material UI (MUI): Styling 
    - moment.js: Used for dealing with datetime data
    - Victory: Used for making charts
- **Module bundler**: webpack

# Back End

- **Programming Language**: Python
- **Web Framework**: Flask
    - Helps develop REST API endpoints.
- **Third Party Libraries**: see our `requirements.txt` file. Some important libraries include:
    - boto3: Used in `model.py` for interfacing with AWS database
    - psycopg2: Used in `model.py` to make PostgreSQL queries

# Database

- **Database Language**: PostgreSQL

# Cloud Deployment

- **Containerization**: Docker / AWS Elastic Container Registry 
- **Load Balancing**: Elastic Beanstalk, Elastic Compute Cloud
- **Security**: AWS Web Application Firewall

