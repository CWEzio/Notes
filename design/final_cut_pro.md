# Usage

# Problem
## FCP stop responding when open the project
### Case 1
- This issue can be caused by network connectivity. In my case, a project effect seemed to depend on an online connection. Without internet access, Final Cut Pro became unresponsive when opening the project. Enabling a global VPN resolved the issue.

## Project gets too large
- During the process of video editing, FCP would create some render files that take a lot of space, which can be safely deleted. To delete them, 
  - First select the project in the browser.
  - Then, in `File` -> `Delete Generated Event Files`.
  - In the pop window, check `Render File`.