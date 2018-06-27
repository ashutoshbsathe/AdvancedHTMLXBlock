# AdvancedHTMLXBlock
Advanced HTML XBlock for OpenEdx(https://github.com/edx) [Beta]

### Features :
- Full CSS support
- Live Preview HTML
- Code Indentation
- Autocomplete Tags
- Autocomplete Brackets

### Installation :
1. Clone this repository on your machine. Testing this first on devstack is always recommended
2. Enter the shell of your devstack and execute :
    ```
    sudo -u edxapp /edx/bin/pip.edxapp install /path/to/cloned/directory
    ```
    Note: You need to point pip to the directory containing `setup.py` of this project. For example: If you cloned this repo in directory called `/home/edx/advhtmlxblock` and `/home/edx/advhtmlxblock/setup.py` is present then `/home/edx/advhtmlxblock` is your required path
3. [Re]start your LMS and CMS.
4. Login to studio as staff
5. Go to "Advanced Settings" in your course
6. Add word `advancedhtml` to list of "Advanced Modules"
7. Save changes 
8. Advanced HTML component should be present in "Advanced" section in your course.

***Special Note for Docker based devstack :***
Since the docker based devstack has 2 separate containers for studio and lms, you have to install your xblock in both the containers and then restart both the containers. The workflow would look something like this :
1. `$ make studio-shell`
2. Install the xblock as mentioned above
3. `# exit` from studio-shell
4. `$ make lms-shell`
5. Install the xblock as mentioned above
6. `# exit` from lms-shell
7. `$ make studio-restart && make lms-restart`



### Introduction :
OpenEdx's current HTML component does not allow you add internal and/or external CSS(via \<link\>), also the raw HTML editor does nt save your indentation and shows a dirty minified coe.
Even if you add <style> tag in OpenEdx html component and try to theme basic elements, the CSS will spill all over the page as shown [here](https://imgur.com/a/v1imOMd)

### So why does this happen ?
The HTML component uses very old versions of editors TinyMCE and CodeMirror which did not support code indentation by default. Whatever content received from editors is put directly into the course without checking any tags and that is why styles spill all over the page

### How does AdvancedHTMLXblock work ?
AdvancedHTMLXBlock essentially extends the raw HTML component of OpenEdx. This XBlock uses the latest version of CodeMirror(5.38 as of June 2018).
Editor is configured to enable code folding/code indentation etc. All the html content received from the editor is then put into an iframe.
The height of the iframe is chanegd on changing html content and iframe is styled so that it looks virtually absent.