AI-Based IDS for IOT Networks

(Cyber Physical Systems Project)

Youssef Mahmoud Elsaeed,

Mohamed Hossam Elshaarawy,

Youssef Moataz,

Youssef Darwish,

First of all, we need to identify our project scope and it's
requirements so let's start with the problem statement:

- The growth of IoT and Cyber-Physical Systems (CPS) has created
  security challenges due to limited device resources and large-scale
  connectivity. Traditional security mechanisms often struggle to detect
  sophisticated or unknown attacks.

- Two problems our project solves:

  - Adaptability to New Attacks: Traditional systems use \"rules\"
    (e.g., \"Block IP 1.2.3.4\"). If an attacker changes their IP, the
    rule fails. Our AI project solves this by learning behavior
    patterns, so it can detect attacks it hasn\'t explicitly seen before
    (Zero-Day attacks).

  - Resource Constraints in IoT: Standard security software is heavy and
    drains battery/CPU. An AI-based IDS (especially one optimized like
    the one we are building) can analyze network traffic efficiently
    without needing to be installed on every tiny sensor, acting as a
    centralized guard.

The goal of our project:

- Design and analysis of an Intrusion Detection System (IDS) that uses
  Machine Learning or Deep Learning to detect malicious activities in
  IoT networks.

- Specific attacks targeted or examples of it mapped to CIA triad:

  - Denial-of-service. (availability)

  - Man in the middle. (Integrity, Confidentiality)

  - Eavesdropping. (Confidentiality)

  - Data Injection Attacks. (Integrity)

Requirements:

- System Modeling: Model a typical IoT system architecture and identify
  where potential attacks might occur.

- Attack Analysis: Analyze at least two types of cyber-attacks that
  affect IoT systems.

- IDS Design: Design an AI-based IDS and explain the workflow of how it
  detects intrusions.

- Evaluation: Define the metrics you will use to measure success, such
  as accuracy, detection rate, and false positives.

- Discussion: Discuss the advantages and limitations of your solution,
  along with potential future improvements.

Now let's go for the Dataset choosing:

- First we've made our research for the top tier IOT attacks datasets
  available publicly.

- We've reached out two options:

  - CIC_2023_IOT

    - It is new and massive, covering 33 types of attacks including
      Mirai and Spoofing.

  - EDGE_IOTTset

    - This is a comprehensive, professional dataset created specifically
      for IoT/IIoT cybersecurity. It contains 14 types of attacks mapped
      exactly to our needs.

    - Why?

      - Because it explicitly cover the 4 attack categories that were
        required in the project.

So our Choice is: EDGE_IOTTset.

Some Definitions, Comparisons, and Choices:

What is an iot network?

- An IoT (Internet of Things) Network is a system of interrelated
  computing devices (sensors, smart locks, cameras, industrial machines)
  that transfer data over a network without requiring human-to-human or
  human-to-computer interaction. Unlike a standard office network of
  powerful laptops, an IoT network is heterogeneous (many different
  types of devices and protocols mixed together).

What are iot attacks?

- IoT attacks are malicious attempts to compromise these devices.
  Because IoT devices are often \"always on\" and less secure, they are
  easy targets.

- Nature of the Attacks: In our context, these attacks manifest as
  Network Traffic.

  - Example: A normal thermometer sends 1 packet of data every minute. A
    DoS attack might make it send 10,000 packets a second to crash the
    server.

  - The Project\'s Job: our project analyzes this \"traffic\" (the flow
    of data packets) to spot these anomalies.

What is an Ai-Based IDS?

- An IDS is like a security guard that watches network traffic. An
  AI-Based IDS is a \"smart\" guard. Instead of just memorizing a list
  of known criminals (Rule-Based), it learns what \"normal\" behavior
  looks like and spots anything that looks suspicious (Anomaly-Based).

What is a Dataset (In our context of Ai-Based IDS for iot)?

- It is a giant Excel sheet (CSV) where every row represents one
  communication session (a flow) between devices, and a column labeled
  \"Label\" tells the AI if that specific row was an attack or normal.

Raw Data (PCAP) vs Preprocessed (CSV)

- Raw PCAP: Contains the actual packet bytes. To use this, we would need
  to build a complex \"Deep Learning\" model (like a CNN) that reads
  binary bytes.

- Preprocessed CSV: Contains \"features\" extracted from the packets
  (e.g., Packet Duration, Source IP, Destination Port, Protocol, Flow
  Bytes/Sec). Ready to be loaded immediately.

So Our choice is: CSV (semi Preprocessed)

- For the \"Simulation\" Deliverable: We can simulate \"live\" traffic
  by reading rows from the CSV file one by one, rather than replaying
  actual network packets, which requires complex network drivers.

As we discussed earlier we chose EDGE_IOTTset for this project because
it covers our project but let's illustrate more about it's coverage:

- Edge-IIoTset was created using real modern IoT devices (flame sensors,
  heart rate sensors, etc.).

- It contains real traffic logs for the 4 specified example attacks:

  - DoS: Contains TCP SYN Flood, UDP Flood, HTTP Flood, and DDoS.

  - MITM: Contains ARP Spoofing and DNS Spoofing (which allow
    eavesdropping).

  - Data Injection: Explicitly contains SQL Injection and XSS
    (Cross-Site Scripting).

  - Eavesdropping: Covered under Information Gathering (Port Scanning,
    Vulnerability Scanning) and MITM.

So we've downloaded the [CSV
version](https://www.kaggle.com/datasets/sibasispradhan/edge-iiotset-dataset?resource=download)
of it.

Now to the simplest form of the project:

- Raw Material: The CSV file we downloaded.

- The Machine: The AI Model.

- Product: A decision (is this network traffic \"Normal\" or an
  \"Attack\"?).

let's discuss the algorithm:

The method we're going to build this ai model based on.

Since IoT devices have limited power, we need an algorithm that is smart
but not too heavy, and Since we are working with the Edge-IIoTset which
is a CSV file of traffic flows, so our best-fit choice were:

1D-CNN layers + DNN/MLP layers

- What is DNN?

  - This is the standard \"Deep Learning\" model. It consists of an
    Input Layer, several \"Hidden Layers\" of neurons stacked on top of
    each other, and an Output Layer. It is often called a Multi-Layer
    Perceptron (MLP).

  - How it works: It takes a single row of your data (one network flow),
    passes it through 3-5 layers of math to extract complex features,
    and outputs the attack type.

  - Why?

    - Simplicity: It works directly with the CSV data format we already
      have. We won't need to do complex data reshaping.

    - Performance: It is generally Good.

- What is CNN?

  - CNNs is used in image recognition (2D). A 1D-CNN is a version
    designed for text or signals. It slides a \"filter\" across your
    data row to find local patterns.

  - How it works: It looks at groups of features together (e.g.,
    \"Protocol\" + \"Packet Size\" + \"Duration\") to see if they form a
    suspicious shape.

  - Why?

    - Speed: It is very lightweight and fast to train.

    - Feature Extraction: It is excellent at automatically figuring out
      which columns in our CSV matter the most.

- So, the architecture is: Input -\> CNN (Scan) -\> DNN (Decide) -\>
  Output

Architecture: Input -\> CNN (Scan) -\> DNN (Decide) -\> Output

- The \"Eye and Brain\" Analogy

  - Think of how you recognize a car:

  - The Eye (CNN Layers): Your eye scans the object. It sees \"circular
    shapes\" (wheels), \"glass rectangles\" (windows), and \"metal
    curves.\" It doesn\'t know it\'s a car yet; it just extracts the
    features.

    - In our project: The 1D-CNN scans the raw numbers in our CSV and
      finds patterns (e.g., \"This packet size is rising while the
      duration is falling\").

  - The Brain (DNN Layers): Your brain takes those features (\"wheels +
    windows + metal\") and decides: \"That is a Car.\"

    - In our project: The DNN (Dense Layers) takes the patterns found by
      the CNN and makes the final decision: \"This is a DoS Attack.\"

- Why this is SMART?

  - Because DNN alone is short for patterns, it can't recognize patterns
    standalone by itself, but with the help of CNN it can.

So here's the breakdown of the architecture:

1.  Input Layer: Receives the packet features.

2.  1D-CNN Layer: Scans the features to extract patterns.

3.  Pooling Layer: Shrinks the data to keep only the important info.

4.  Flatten Layer: Prepares data for the final decision.

5.  Dense Layers (MLP): Makes the final classification
    (DoS/MITM/Normal).

6.  Output Layer: Gives the final answer.

Implementation:

- Here's a screenshot of the CSVs after extracting the dataset:

  - ![A screenshot of a clock AI-generated content may be
    incorrect.](03-Security-Infrastructure-Deployment\AI-IDS-IoT\images/media/image1.png){width="7.067279090113736in"
    height="1.0000863954505688in"}

  - DNN-EdgeIIoT-dataset.csv: It contains more data and features than
    the others, The one we'll use for training.

  - live_data_training.csv: This is for testing the model in real-time,
    not for training.

  - ML-EdgeIIoT-dataset.csv: is negligible, it's for classical machine
    learning algorithms.

Preprocessing: Four Phases of (What & Why)

1.  Data Cleaning (The Trash Collection):

    - What: Removing columns that confuse the AI.

    - Why: The dataset has columns like ip.src_host (Source IP) and
      frame.time (Timestamp). If we leave \"IP 192.168.1.5\" in, the AI
      memorizes \"192.168.1.5 = Bad Guy.\" If the hacker changes their
      IP, the AI fails. We want it to learn behavior (Packet Size,
      Duration), not identity.

2.  Label Encoding (The Translation):

    - What: Converting the \"Answers\" (Target column) from text to
      numbers.

    - Why: The AI cannot output \"DoS Attack.\" It outputs Class 3. We
      need to assign: Normal=0, Backdoor=1, DoS=2, etc.

3.  Feature Scaling (The Equalizer):

    - What: Squashing all numbers into a small range (usually -1 to 1).

    - Why: In your data, Packet_Size might be 60,000 bytes, but Duration
      is 0.001 seconds. If you don\'t scale, the AI will think
      Packet_Size is a million times more important just because the
      number is bigger. StandardScaler fixes this.

4.  Reshaping (The 3D Box for CNN):

    - What: Converting the 2D Excel sheet into a 3D format.

    - Why: A 1D-CNN (Convolutional Neural Network) expects to \"slide\"
      over the data. It needs the data shape to be (Number_of_Rows,
      Number_of_Columns, 1).

5.  \"Grouping\" logic: (Not a phase)

    - It groups DDoS_UDP, DDoS_TCP -\> into \"DoS\"

    - It groups XSS, SQL_Injection -\> into \"Injection\".

    - It keeps everything else (Ransomware, Backdoor, etc.) as they are.

They'll be illustrated in the code by the same order and the naming
conventions for you doctor Ashraf to be able to understand them easily.

Now we can start our code building block by block, and after we finish
we can run into our conclusion and metrics:

- Accuracy: The percentage of total correct guesses.

  - Warning: In security, this is misleading. If 99% of traffic is
    normal, a dumb model that never detects attacks still gets 99%
    accuracy.

- Precision: \"When the AI screams \'ATTACK\', is it actually an
  attack?\"

  - High Precision = Few False Alarms.

- Recall (Detection Rate): \"Out of all the attacks that happened, how
  many did the AI find?\"

  - High Recall = You are safe (you didn\'t miss the hackers).

- F1-Score: The balance between Precision and Recall.

- Loss: A math score for the AI. It should go down over time. If it says
  nan, the data is broken. If it is 0.00, the model is suspicious
  (overfitting). A good loss is usually between 0.1 and 0.01.

System Evaluation:

![A black screen with white text AI-generated content may be
incorrect.](03-Security-Infrastructure-Deployment\AI-IDS-IoT\images/media/image2.png){width="7.5in"
height="1.9944444444444445in"}

Overview:![A screenshot of a computer AI-generated content may be
incorrect.](03-Security-Infrastructure-Deployment\AI-IDS-IoT\images/media/image3.png){width="7.5in"
height="6.65625in"}

![A screenshot of a calendar AI-generated content may be
incorrect.](03-Security-Infrastructure-Deployment\AI-IDS-IoT\images/media/image4.png){width="7.5in"
height="3.68125in"}

A. System Performance Overview

- Final Test Accuracy: 94.42%

  - ![A black background with white text AI-generated content may be
    incorrect.](03-Security-Infrastructure-Deployment\AI-IDS-IoT\images/media/image5.png){width="7.5in"
    height="1.4590277777777778in"}

- Loss: 0.1403 (Low loss indicates the model is stable and confident).

- Training Time: \~21 seconds per epoch (Very efficient for a 1.1GB
  dataset).

B. Key Strengths

- Zero False Positives: The model achieved a Precision of 1.00 for
  Normal traffic. This ensures legitimate IoT operations are never
  interrupted by false alarms.

- High DoS Detection: With an F1-Score of 0.92, the system effectively
  mitigates the most frequent threat to IoT networks (Denial of
  Service).

- Robust Feature Extraction: The 1D-CNN layers successfully learned to
  distinguish attack traffic patterns from normal behavior without
  needing manual rule-setting.

C. Limitations & Challenges (The \"Smart\" Section)

- Class Imbalance: Rare attack types like *Ransomware* and
  *Fingerprinting* constituted less than 1% of the dataset. As a result,
  the model sometimes misclassified these as broader categories like
  \"Injection.\"

- Future Improvements: To fix the specific labeling of rare attacks, we
  recommend SMOTE (Synthetic Minority Over-sampling Technique) to
  artificially generate more Ransomware training data in future
  versions.
