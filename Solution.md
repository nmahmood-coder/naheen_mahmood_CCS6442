# Solution – Git Workflow Justifications

This file explains my decisions for requirements 6, 19, 21, and 22.

## Requirement 6 – Problem pushing to GitHub

When I tried to push my changes in GitKraken, I received a message saying that my local `developer` branch was behind `origin/develop`. This meant that the remote branch had changes that I did not have locally, so Git would not let me push.

To fix this, I needed to **fetch and pull** the latest remote changes, resolve any conflicts, and then push again. I avoided **Force Push** because it could overwrite someone else's work. I could have avoided this problem by pulling regularly and checking the commit graph before pushing.

The assignment also asked for screenshots of the GitKraken commit graph before and after fixing the issue. These have been added to the repository and committed. The warning message alone is not a substitute for the two graph screenshots.


## Requirement 19 – Fix review issues and create a new Pull Request

I first left feedback explaining why the earlier Pull Request was not ready to merge. I then addressed the issues I identified, such as keeping the changes focused, using clear commit messages, updating the branch, and checking for conflicts and errors.

I created a new Pull Request targeting the development branch so the corrected work could be reviewed before merging. This is better than merging unfinished or unchecked changes directly. Since I created the Pull Request myself, I left my feedback as a comment rather than approving my own work.


## Requirement 21 – Release Version 1

The Project Manager wanted us to deliver only the code that was ready for customers. I used a release branch so I could prepare and test Version 1 without including unfinished features.

After checking the release, I merged it into the production branch (`master`) and kept the development branch updated. I created a version tag (`1.0.0`) to mark the released code. I then pushed the changes and published the release on GitHub, with release notes and any files the customer needed.

This approach keeps the production version stable and makes it easy to find the exact code delivered to the customer.


## Requirement 22 – Add a new method to `User`

The customer asked for a new feature after Version 1 was released. I used a feature branch based on the develop branch because this was new functionality, not an urgent production bug.

I added this method to the `User` class in `user.py`:

```python
def __str__(self):
    return f"name: {self.username}, bio: {self.bio}"
```

The method makes a `User` object easier to read when printed. I tested the change, committed it, and prepared it for review through a Pull Request to the develop branch. Keeping it on a feature branch prevents the new work from changing the already released Version 1.


Part 2 – Docker Compose, Conflict Resolution, and Production Release

Docker Compose Integration

I used feature branches to make the Docker changes without affecting the main development branch. I configured three separate services: app for FastAPI, postgres for the database, and checkstatus for checking database availability.

I tested the services using Docker Compose. FastAPI was running, PostgreSQL was healthy, and checkstatus completed successfully with exit code 0.

Conflict Resolution

My feature branch initially did not include the latest Docker Compose changes from develop. I fetched the remote changes and merged origin/develop into my branch.

Git completed a fast-forward merge without conflicts. This allowed me to keep the latest changes without overwriting existing work or using a force push.

I also fixed a Windows line-ending issue in checkstatus.sh by converting it to Unix LF format and adding a .gitattributes rule to prevent the problem from happening again.

Production Image Versions

I initially updated Production-Dockerfile to use citycase/assessment2:1.0. When I tested the image, it displayed ERROR VERSION.

I checked the available tags on Docker Hub and found that version 2.0 was the latest numbered version. I tested citycase/assessment2:2.0, which displayed LATEST VERSION and working docker.

The updated image used Alpine 3.16, which did not support the postgresql16-client package. I replaced it with postgresql14-client so the Docker image could build successfully and run the database readiness check.

Release and Hotfix Decisions

I kept the original production-image Pull Request open while correcting the outdated image. I updated the same feature branch instead of creating a new hotfix branch because version 1.0 had not yet been merged into production through that Pull Request.

This prevented the outdated image from being released and kept the related changes together for review.

I would use a hotfix branch from master if an urgent problem had already reached production. For this task, correcting the image before merging was the safer approach.

I kept the Docker image version separate from the application's release version. The application had previously been released as 1.0.0, while citycase/assessment2:2.0 identifies the corrected Docker image.