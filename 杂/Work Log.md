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

3. When the system is freshly restarted, here is how to start the EIS.
   1. If docker-compose.yml is modified, run <mark>` $ sudo ./provision_eis.sh ../docker-compose.yml`</mark>.
   2. Run command `$ xhost +` before starting EIS stack. This is needed by `ia_visualizer` service to render the UI.
   3. Run `docker-compose up --build -d`. Or run the two steps like below:
      ```
      $ docker-compose build
      $ docker-compose up -d
      ```

4. I have tried other video files, ip camera, usb camera, webcam, and never succeeded. There is another option: use the <mark>camera simulation</mark> as stated in the user guide.
![](rtsp_simulation.png)
Unfortunately this approach is failing. Need to confirm again and then troubleshoot it.
![](rtsp_sim_failed.png)


# 3 Feb 2020 Monday

## EIS user guide readthrough

### Video Ingestion -> filter

The <mark>Filter</mark> (**user defined function**) is responsible for doing pre-processing of the ingested video frames. It uses the filter configuration (in **etcd_pre_load.json**) to do the *selection of key frames* (frames of interest for further processing).

PCB filter checks the ingested video once **every 8 frames**. If in this 8th frame, the PCB is **in the center of the frame**, the data will be added to the output queue.

> We can use the same technique to do the pre-processing for the A1 project. If the boundary marker is in view of the camera, the frame data is ready to be passed to the next module to do further analysis. ![](boundary_marker.png)

`training_mode` key in the filter configuration. </br>
`training_mode: If "true", used to capture images for training and building model`.</br>
> __**Need explanations on how to do training and building model.**__ Can the training only be done with OpenVINO? What does openVINO has to do with **training_mode** key in the filter configuration? In the configuration for Video Analytics, "HDDL" and "MYRIAD" devices require different model files than "CPU" and "GPU" devices.

### Video Analytics

Video Analytics module subscribes to the published input stream coming out of Video Ingestion module over messagebus to get (metadata, frame) tuple.

The <mark>Classifier</mark> (**user defined function**) is responsible for running the classification algorithm on the video frames received from filter.

The PCB demo sample application uses both computer vision and deep learning algorithms for detecting defects on the PCB board.

The defects detected in the demo are missing components and shorts. We can modify this part to cater to our need to detect missing gear parts at the assembly platform.

### Factory Control App

Different modules in this software communicate through a publisher-subscriber mode. This factory control app subscribes to the output from the Video Analytics module, and controls the alarm light and reset button accordingly. 

We can refer to this app to develop our notification mechanism if abnormality is detected.

