# Contributing to this repository

First off, thanks for taking the time to contribute! The following is a set of guidelines for contributing to CLADE project. We encourage everyone to follow them with their best judgement.

## Table of contents

1) [Issues](#issues)
2) [Contribute](#contribute)
3) [Development](#development)

## Issues

With issues you can report bugs, issues or suggestions, to improve the project. A set of rules have also been established in the issues, shown below:

- **Before opening an issue, check that the topic is not being treated in another**. Do a bit of research
in the open and closed issues to try not to repeat topics. When searching, **good labeling** can be key.
- **Try to keep the title simple and concise**. So that at first view you can get an idea of ​​the subject.
- **Make an effort to try to explain the reason for the issue in the description**. The description must contain all the important information associated with that topic.
- **Associate similar or dependent issues in the description or in comments**. Both by the creator and collaborators, using `#issue-number`.
- **Issues are closed when they have been solved/invalidated or repeated**. They can be closed by the creator or by the members of the repository when the issue has **been resolved**, after discussing it and, if necessary, applying changes.

## Contribute

 1. Review current open issues and PRs.
 2. If there is no one with your proporsal or bug, open an Issue.
 3. Fork the repository and clone it to your local machine.
 4. Make the necessary changes, following the feedback given in the issue.
 5. Check that the tests pass and, if necessary, add the necessary tests for your new functionality.
 6. Add documentation.
 7. Open a pull request with a description of your changes and a link to the corresponding issue.
 8. If approved, we will merge it into the main branch, otherwise you will need to make further changes.

## Development

Requires Python 3.10 and the libraries in the ``requirements.txt`` file. Documentation can be found in the ``README.md`` file and in the ``docs`` folder.

All tests must be passed before submitting a PR. We use Pytest, you can learn more about it in the [official documentation](https://docs.pytest.org/en/8.2.x/), and run tests with it:

````
pytest test
````