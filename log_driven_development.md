# Log-Driven Development
https://www.infoworld.com/article/3017687/application-development/get-started-with-log-driven-development.html

## Before pushing code to production you must have
- unit tests
- integration tests
- descriptive log messages
- comprehensive metrics
  - numerical data
    - system error count
    - failure count
    - user download count
  - log data
    - detailed and descriptive
    - types of potential system errors
    - types of potential exceptions
    - types of potential warnings
    - possible unpredictable user experiences ('never happen')
- alerts on critical log messages
- monitoring dashboards within the troubleshooting platform

## Balance creativity with accountability
- responsibility
  - you develop and push a feature to production == you are accountable for the outcome of that feature
  - whether it is successful or not, responsibility for the performance of the feature rests with the developer
- recognition
  - you successfully implement a feature == you own and are recognised for that

## Writing logs
- **GDPR:** no sensitive, personal identifiable information
- **Security:** no business critical information
- **Performance:** look out for potentially high CPU/memory/disk usage
- variables in logs must be human-readable or what is the point

## Potential Issues
- humans reading logfiles is not efficient
- you will run out of space if you log everything
- knowing what and how to log is something best discussed with a devops expert and a support expert together
