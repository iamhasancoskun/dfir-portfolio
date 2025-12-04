# Lespion: OSINT Analysis of an Insider Threat Case

![alt text](https://img.shields.io/badge/Category-OSINT%20Investigation-blue) ![alt text](https://img.shields.io/badge/Platform-CyberDefenders-green) ![alt text](https://img.shields.io/badge/Discipline-Insider%20Threat%20Analysis-red) ![alt text](https://img.shields.io/badge/Author-iamhasancoskun-black)

**Author:** Hasan Coşkun  
**Date:** August 30, 2025  
**Platform:** CyberDefenders  
**Case:** Lespion (OSINT Investigation)  
**Category:** Insider Threat Analysis

---

## 📋 **Case Overview**

This case requires us to track the digital footprints of an insider threat suspect using OSINT (Open Source Intelligence) techniques. Our evidence consists of a GitHub profile believed to belong to an employee and various images. Our mission is to use these open sources to:

- 🔑 Find leaked API keys and passwords
- 🕵️ Connect the suspect's online profiles  
- 🌍 Perform physical location identification
- 💰 Uncover evidence of resource misuse

---

## 🎯 **Evidence Analysis**

### **Primary Evidence Sources:**
- **GitHub Profile:** `https://github.com/EMarseille99`
- **Visual Evidence:** `office.jpg`, `WebCam.png`
- **Repository Contents:** Various code files and configurations

---

## 🔍 **Question 1 & 2: Secrets Embedded in Code**

### **Question 1:** What API key did the insider add to his GitHub repositories?  
### **Question 2:** What plaintext password did the insider add to his GitHub repositories?

### **Analytical Approach:**
One of the most common mistakes a developer can make is to embed sensitive information such as API keys or passwords ("secrets") directly into code and send them to a public repository. Our task is to scan the suspect's GitHub repositories for such leaks.

### **Evidence Examination:**

#### **1. API Key Detection:**
We examine the suspect's `Project-Build---Custom-Login-Page` repository. Looking at the `Login Page.js` file in the repository, we see a long string of characters embedded in a variable called `API Key`.

![Leaked API Key](evidences/q1.png)

**Discovered API Key:** `aJFRaLHjMXvYZgLPwiJkroYLGRkNBW`

#### **2. Password Detection:**
Continuing to examine the `Login Page.js` file in the same repository, we notice a text labeled as `Password` and encoded with **Base64**: `UGljYXNzb0JhZ3VldHRlOTk=`

![Base64 Encoded Password](evidences/q2.1.png)

**Base64 Encoded String:** `UGljYXNzb0JhZ3VldHRlOTk=`

When we decode this text using a Base64 decoder:

![Decoded Password](evidences/q2.2.png)

**Decoded Plaintext Password:** `PicassoBaguette99`

---

## 💰 **Question 3: Cryptocurrency Mining Tool**

### **Question 3:** What cryptocurrency mining tool did the insider use?

### **Analytical Approach:**
An insider threat can misuse company resources for personal gain. Cryptocurrency mining is a common example of such misuse. We should scan the suspect's GitHub repositories for clues related to such tools.

### **Evidence Examination:**
When we examine the suspect's repository list on GitHub, we see a repository "forked" (copied to their own account) from `xmrig/xmrig`.

![XMRig Repository](evidences/q3.png)

**What is XMRig?**
- CPU/GPU mining software used for cryptocurrencies like Monero (XMR)
- Potential for misusing company resources for crypto mining
- High CPU/GPU usage that can degrade system performance

**Discovered Cryptocurrency Mining Tool:** `XMRig`

---

## 🌐 **Question 4 & 5: Tracking Digital Footprints**

### **Question 4:** On which gaming website did the insider have an account?  
### **Question 5:** What is the link to the insider Instagram profile?

### **Analytical Approach:**
People usually use the same or very similar usernames on different platforms. Based on this assumption, we will search for the username `EMarseille99` on other platforms using OSINT tools and search engines.

### **Evidence Examination:**

#### **1. Username Search Results:**
When we search for the username `EMarseille99` using a username search engine like [WhatsMyName.app](https://whatsmyname.app/) or directly on Google, we find matching profiles on multiple platforms.

![Username Search Results](evidences/q4(differentway).png)

#### **2. Platform Matches:**
Search results clearly show that the suspect has accounts on the popular gaming platform **Steam** and social media platform **Instagram** with the same username.

![Google Search Results](evidences/q4-q5.png)

**Discovered Platforms:**
- **Gaming Website:** `Steam`
- **Instagram Profile:** `https://www.instagram.com/emarseille99/`

---

## 🌍 **Question 6, 7 & 8: Location Identification through Image Analysis (GEOINT)**

### **Question 6:** Which country did the insider visit on her holiday?  
### **Question 7:** Which city does the insider family live in?  
### **Question 8:** Which city is the company located in?

### **Analytical Approach:**
These questions test our ability to extract geographic location information from images (GEOINT - Geospatial Intelligence). We will analyze images from the suspect's Instagram profile and case files using reverse image search tools like Google Lens.

### **Evidence Examination:**

#### **1. Holiday Country (Question 6):**
In the suspect's Instagram profile, there is a photo of **Marina Bay Sands**, the iconic structure of Singapore.

![Singapore - Marina Bay Sands](evidences/q6.1.png)

Verification with Google Lens:

![Google Lens - Marina Bay Sands](evidences/q6.2.png)

**Holiday Country:** `Singapore`

#### **2. Family's City (Question 7):**
In another photo from the Instagram profile, we notice a QR code and in the background, **Burj Khalifa**, the world's tallest building. Additionally, when we check the flag in the image containing family-related sharing text through Google Lens, it turns out to be the United Arab Emirates. Also, Burj Khalifa is located in Dubai.

![Dubai - Burj Khalifa](evidences/q7.1.png)

Burj Khalifa detail:

![Dubai - Burj Khalifa](evidences/q7.2(Burj%20Khalifa%20in%20Dubai).png)

**Family's City:** `Dubai`

#### **3. Company Location (Question 8):**
The `office.jpg` image provided in the case files shows a train station and its surroundings with distinctive architecture. When we search this image with Google Lens, we understand that the structure is **Birmingham New Street** train station in England.

![Birmingham New Street Station](evidences/q8.png)

**Company Location:** `Birmingham`

---

## 📹 **Question 9: IP Camera Location**

### **Question 9:** Our intelligence team spotted the target with this IP camera. Which state is this camera in?

### **Analytical Approach:**
For the final question, we need to analyze the `Webcam.png` image in the case files. The image shows a live camera view of what appears to be a university campus.

### **Evidence Examination:**
The image contains "A View from the Dome" at the top and "EarthCam" at the bottom right. When we search for these clues and the image on Google, we determine that this view belongs to the live camera broadcast of **University of Notre Dame** in the United States.

![Notre Dame University](evidences/q9.png)

**University of Notre Dame Location:**
- **University:** University of Notre Dame
- **City:** South Bend
- **State:** Indiana
- **Country:** United States

**IP Camera State:** `Indiana`

---

## 🔗 **OSINT Tools and Techniques**

### **Tools Used:**
1. **GitHub Analysis:** Repository cloning, code scanning, credential detection
2. **Google Search:** Username enumeration, platform discovery
3. **Google Lens:** Reverse image search, location identification
4. **Base64 Decoder:** Encoded credential extraction
5. **WhatsMyName.app:** Username search across platforms

### **OSINT Techniques:**
- **Username Enumeration:** Searching the same username across different platforms
- **Image Analysis:** Extracting geographic and contextual information from images
- **Code Review:** Detecting sensitive information within code
- **Geolocation:** Location identification from images
- **Cross-Platform Correlation:** Combining information from different platforms

---

## 📊 **Findings Summary**

| Question | Answer | Evidence Type |
|----------|--------|---------------|
| **Q1:** API Key | `aJFRaLHjMXvYZgLPwiJkroYLGRkNBW` | Code Analysis |
| **Q2:** Plaintext Password | `PicassoBaguette99` | Base64 Decoding |
| **Q3:** Crypto Mining Tool | `XMRig` | Repository Analysis |
| **Q4:** Gaming Website | `Steam` | Username Enumeration |
| **Q5:** Instagram Profile | `https://www.instagram.com/emarseille99/` | Social Media Discovery |
| **Q6:** Holiday Country | `Singapore` | Image Analysis (Marina Bay Sands) |
| **Q7:** Family City | `Dubai` | Image Analysis (Burj Khalifa) |
| **Q8:** Company Location | `Birmingham` | Image Analysis (Birmingham New Street) |
| **Q9:** IP Camera State | `Indiana` | Image Analysis (Notre Dame University) |

---

## 🎯 **Insider Threat Indicators**

### **1. Credential Exposure:**
- API keys hardcoded in code
- Base64 encoded passwords stored in repositories
- Sensitive information committed to public repositories

### **2. Resource Misuse:**
- Forking cryptocurrency mining tools (XMRig)
- Potential for misusing company resources for personal gain

### **3. Digital Footprint:**
- Consistent username usage across different platforms
- Trackable social media activities

---

## 🧠 **Lessons Learned**

### **OSINT Investigation Best Practices:**
1. **Systematic Approach:** Systematic analysis for each evidence type
2. **Tool Integration:** Combining different OSINT tools
3. **Cross-Validation:** Validating findings with multiple sources
4. **Pattern Recognition:** Detecting user behavior patterns

### **Insider Threat Detection:**
1. **Code Review:** Detecting sensitive information within code
2. **Repository Monitoring:** Credential exposure in public repositories
3. **Resource Usage:** Detecting misuse of company resources
4. **Digital Footprint:** Monitoring employees' online activities

---

## 🚀 **Next Steps**

### **Investigation Continuation:**
1. **Additional Repository Analysis:** Searching for more credentials in other repositories
2. **Social Media Deep Dive:** More location information in Instagram profile
3. **Timeline Analysis:** Creating activity timeline
4. **Network Analysis:** Mapping the suspect's digital network

### **Prevention Measures:**
1. **Code Review Policies:** Preventing storage of sensitive information in code
2. **Repository Security:** Credential scanning in public repositories
3. **Employee Monitoring:** Monitoring usage of company resources
4. **Security Training:** Security awareness training for developers

---

## 🏆 **Conclusion**

This case perfectly demonstrates how powerful OSINT can be in a cybersecurity investigation. Starting from a single GitHub profile, we successfully:

✅ **Code Leaks:** Leaked API key and password to public code repositories  
✅ **Resource Misuse:** Showed interest in cryptocurrency mining tools  
✅ **Digital Identity:** Used the same username across different social media and gaming platforms  
✅ **Physical Locations:** Identified cities where they went on vacation, where their family lives, and where they work through Instagram posts and other images  

This success proves how different types of digital evidence can be brought together to create a comprehensive intelligence picture and demonstrates the critical role of OSINT techniques in insider threat investigation.

---

## 📚 **Resources and References**

- **GitHub Profile:** https://github.com/EMarseille99
- **Instagram Profile:** https://www.instagram.com/emarseille99/
- **OSINT Tools:** WhatsMyName.app, Google Lens, Base64 Decoder
- **Platforms:** Steam, Instagram, GitHub
- **Locations:** Singapore, Dubai, Birmingham, Indiana

---

*This case analysis contains the solution to the Lespion challenge on the CyberDefenders platform. All findings and analyses are prepared for educational purposes.*
