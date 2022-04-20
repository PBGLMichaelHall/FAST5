# FAST5
# Tree Directory for processing and analyzing FAST5 files from a MinION run....

![777](https://user-images.githubusercontent.com/93121277/164006307-9d4b2f23-fe42-49d2-9aa6-cc64f737291b.png)


# First we want to reorgainze the data structure so the RScript from https://github.com/roblanf/minion_qc can plot,interpret and analyze the MinION reads. Create a summary directory and place final_summary.txt and sequencing_summary.txt. Copy the MinIONQC.R Script into main directory and run the following command line. It will produce a standard output if successful. 


![SO](https://user-images.githubusercontent.com/93121277/164192115-6b8a1a95-7a78-41d3-91b1-a40f8a89bcbd.png)


Channel Summary 

![channel_summary](https://user-images.githubusercontent.com/93121277/164192571-f603793d-9c4d-417b-b52d-94ea9a3ecf11.png)

Flowcell Overview
# There are 16 X 32 = (512) Flowcells available, however in the plot 5 - 30 are missing.....

![flowcell_overview](https://user-images.githubusercontent.com/93121277/164192623-17d84cc5-5b28-4b8b-9895-55ffb70f86f9.png)

GB Per Channel Overview
# Again there is missing Channels notice all the white space

![gb_per_channel_overview](https://user-images.githubusercontent.com/93121277/164192665-6681a9b5-e627-4e4e-b279-bb50e9ac2abf.png)

Length By Hour

![length_by_hour](https://user-images.githubusercontent.com/93121277/164192699-b965045b-d31d-4fa8-a4d7-21af69475895.png)

Length Histogram

![length_histogram](https://user-images.githubusercontent.com/93121277/164192732-73f299a8-55e8-4158-a714-869848fc6e40.png)

Length VS. Quality

![length_vs_q](https://user-images.githubusercontent.com/93121277/164192770-462eb9b4-7608-4adf-84a4-25a1e5e6ccf9.png)

Quality by Hour

![q_by_hour](https://user-images.githubusercontent.com/93121277/164192811-cff611ab-f357-4be9-992c-1ebcb117263a.png)

Quality Histogram 

![q_histogram](https://user-images.githubusercontent.com/93121277/164192833-d4cb1298-4c1d-4ebb-847f-7fd0072d35ae.png)

Reads Per Hour

![reads_per_hour](https://user-images.githubusercontent.com/93121277/164192882-6c7e9354-6864-49d3-9e04-d1c8fa81dabb.png)

Yield By Length

![yield_by_length](https://user-images.githubusercontent.com/93121277/164192921-82f8ca45-cca7-4019-a5c9-a289407b23ed.png)

Yield Over Time

![yield_over_time](https://user-images.githubusercontent.com/93121277/164192967-1938c6c2-9b08-416d-b6a6-7897d7ad8af5.png)

# FASTQ Quality Summary for the FAILED Minion Run

![fail1](https://user-images.githubusercontent.com/93121277/164201047-4dd7d379-fba7-47af-a0c9-e48334f09db7.png)

![fail2](https://user-images.githubusercontent.com/93121277/164201041-488485c3-a425-4277-884a-44d0db03362a.png)

# FASTQ Quality Summary for the PASSED Minion Run



![pass1](https://user-images.githubusercontent.com/93121277/164201143-c3431342-9829-4bdc-8ff4-60e9cd494618.png)
![pass2](https://user-images.githubusercontent.com/93121277/164201142-a314f352-04ce-4923-8baa-d7a029683101.png)
![pass3](https://user-images.githubusercontent.com/93121277/164201140-50e91b57-3965-41cd-991b-59ffab8a1050.png)










