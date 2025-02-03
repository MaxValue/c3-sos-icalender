# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2025-02-03

### Added

Initial public version. Provides described functionality. But here are the details:
- Base functionality:
    1. Run a script on the current Safari tab
        1. the script extracts the information for the event and returns it as a dictionary
        2. the shortcut creates an event from the dictionary
        3. the shortcut switches to the calendar app to display the new event
