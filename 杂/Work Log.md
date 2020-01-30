# 29/01/2020 Wednesday

TODO for tomorro:

* reinstall ubuntu in VM ?
* reinstall EIS in ubuntu VM

# 30/01/2020 Thursday

## Visual Computing team meeting (9:00 - 9:30)

My work update:

* Edge insights software
  * testing recognizing and detecting the object
* Inventory app
  * delayed
  * check with Martin of the deadline, (31/3?) and theFET (max 30%)

## Log

1. Reinstalled Ubuntu VM in VirtualBox

   1. By default the screen size of Ubuntu guest os is very small, at about `640 x 480`. Fixed this by attaching the `VBoxGuestAdditions.iso` file to the optical drive, and running the disk from Ubuntu guest os. [link](https://askubuntu.com/a/451825)
   2. Shared a folder between Windows and Ubuntu, but in Ubuntu when I clicked on the folder, message of "You don't have the permissions necessary to view the contents" popped up. This was fixed by running `sudo adduser [your-user-name] vboxsf`. This command add me to the shared folders group in the Ubuntu guest os. [link](https://askubuntu.com/a/890740)

2. Now installing the Edge Insights Software.

   1. The structure of the folders is as below. Last time when I tried out EIS, I was confused about the structure. I was uncertain whether EIS was installed or not, as there was the `IEdgeInsights` folder, but also the `installer` folder. In the end I didn't start from the `installer` folder but the `IEdgeInsights`.

      ```
      . EdgeInsightsSoftware-v2
      +---- Docs
      +---- IEdgeInsights
      +---- installer
      +---- turtlecreek
      +---- README.md
      ```

   2. Now I'm running the `installer\installation\setup.sh` script.

   3. EIS installation failed as the file `./install.sh` is not executable.
      ![](eis_install_failed.png)
      
      Fixed this by adding one line to the original `Dockerfile.eisbase` script.

      ```
      RUN chmod +x ./install.sh  <--- added this line
      RUN ./install.sh --cython
      ```

   4. Another error. The fix:
   
      ```
      COPY . ./VideoIngestion/
      RUN chmod 755 ./VideoIngestion/vi_start.sh   <--- added this line
      ENTRYPOINT ["VideoIngestion/vi_start.sh"]
      ```

   5. <mark>Succeeded!</mark> The `setup.sh` installed all the stuff. When it finished, the EIS Visualizer App automaticlally popped up and the PCB demo is now being played.
      ![](eis_visualizer.png)

