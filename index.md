# ESP32 Weather Station 
Checking the weather on your phone is cool an all but imagine if you had a portable device that can display the humidity and light of the weather exactly where you are. 

You should comment out all portions of your portfolio that you have not completed yet, as well as any instructions:
```HTML 
<!--- This is an HTML comment in Markdown -->
<!--- Anything between these symbols will not render on the published site -->
```

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Jennifer D | Granada Hills Charter Highschool | Electrical Engineering | Incoming Senior

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Book logo](/least-github-pages/assets/logo.png)
  
# Final Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**


For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE



# Second Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

**Technical Details:**
- Connected two ESP32’s in order to transmit data within one another using the esp now library
- Added a photoresistor and a thermistor to the newly added ESP32 in order to get the light intensity and the humidity level and transmit the data collected to the ESP32 receiver to display it onto the OLED


# First Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/bxvZ5RsWZrU?si=I1TUHWwkD7sMzoyL" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

**Surprising Things:**
- During milestone 2 there has been some surprising things that I have noticed. Picking this project has allowed a lot of customizability and I can connect a lot of sensors. 
- Because the ESP32 has wifi and bluetooth there are a lot of doors that open when I am using ESP32’s

**Challenges:**
- Connecting the two ESP32s using the esp now function has been my biggest challenge. In order to connect the two esp32’s together I need to make sure that they are on the same wifi channel. So I set them both to wifi channel 6. 

- Because I am using the esp now function, I do not need wifi to connect the two esp32’s however I do need to connect the receiving esp32 to the wifi. Because the wifi channel is constantly changing on the mobile hotspot I need to make sure both esp32’s is connected to the same wifi channel as the mobile hotspot. 

**Next goals: **
- For my final milestone I plan to use MQTT client which sends my sensor data onto a web server in order for my esp32 receiver to get the data. This is a more sustainable and consistent way to transmit data between my two esp32s rather than using the esp now function which is inconsistent when connecting and takes a few tries to connect the two esp32s. 


The ESP32 weather station utilizes an ESP32 with built-in Wi-Fi to get information from an open source API, OpenWeatherStation, and displays the information on an Oled. 

**My Plan:**
1. First work on the hardware
- Connect the Oled and the ESP32 together using male to female wires, a breadboard, and male wires. 
2. Setup the ESP32 on the Arduino IDE
- Install the library esp32 by Espressif Systems 
- Set up the port and select ESP32 Dev Module
3. Test the Oled 
- Used an image and converted it to byte arrays to display on my Oled
- Using this website: https://javl.github.io/image2cpp/  
4. Set up Wi-Fi on ESP32
- Create a hotspot on a separate device with 2.4 Hz
- Added the name and password of the hotspot 
- Wrote code that displayed the IP address and whether the ESP32 was able to connect to the hotspot or not in the serial monitor. 
5. Got an API Key
- Created an account of OpenWeathermap which is an open source API 
- Got an API key and use it to get data for the temperature and description of the weather in my area
- Parsed the JSON file by storing it into doc and picking out what I wanted to display using float temp and const char description
Displayed the temperature and description onto the Oled

**Challenges:**
1. Connecting to Wi-Fi
- When trying to connect my ESP32 to wifi I first tried to use the hotspot on my phone however that did not work, so I had to use my laptop in order to create a 2.4 Hz mobile hotspot my ESP32 could connect too. I changed the name and password of my hotspot to be simpler so the ESP32 could be connected easier. In order to troubleshoot I made sure to display whether the ESP32 was in the process of connecting and whether it was unable or unable to connect on the serial monitor.
2. HTTP request 
- In order for my Oled to display real time data that frequently changes, I need to send a GET request to get the data. However I did not know how to approach this so with the help of my instructor I was able to store the string from the Json file into the variable payload then gets parsed in the doc object. 
3. Displaying the Temperature and Description on the Oled
- My temperature and description were displayed in the serial monitor however not on my Oled. The problem was because I didn’t set the color  of the text that was going to display on the Oled. So I couldn’t see what was being displayed on my Oled. However when I set the text color to white I was able to see what was being displayed on the Oled. 


# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

# Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

```c++
void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);
  Serial.println("Hello World!");
}

void loop() {
  // put your main code here, to run repeatedly:

}
```

# Bill of Materials
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| ESP32 | Get data from weather station website | $15.19 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/HiLetgo-ESP32-DevKitC-ESP32-WROOM-32U-ESP-WROOM-32U-Development/dp/B09KLS2YB3/ref=sr_1_2_sspa?crid=RZS0VO0FHLVG&dib=eyJ2IjoiMSJ9.UdLxS8engRob9RiEzo8GfUX_eb5SIivviByIHACD0jgBzVFD5MSKwRvt2HMUQ7jFwjmW8ZG0IeuXxh15af8FtOTBMtuZxzmPoijgCAnKnMBABOHZyD0edn4YZkFJbrA5RfjALOAtDYMc95a05cKxR9wKnwQd1YByAgxWGkl5UDc3XCV-nKV2pEM5FC9Wd_ZQUxuXoQkWv5tMsbM1aizGqFFphD4vJO6XYRx27X9L_yI.c3APPMEBNkLendl2C4CcAdNRThGAmdE_-whahkygZ4U&dib_tag=se&keywords=esp32&qid=1750359632&sprefix=esp32%2Caps%2C99&sr=8-2-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1)"> Link </a> |
| Micro USB Port | Provide power to ESP32 | $5.49 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/dp/B098DW7485?ref=nb_sb_ss_w_as-reorder_k0_1_8&amp=&crid=1YRBOW66YBW2H&sprefix=microusb&th=1)"> Link </a> |
| SSD1306 | Displays weather | $6.99 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/HiLetgo-Serial-128X64-Display-Color/dp/B06XRBYJR8/ref=sr_1_3_pp?crid=3OI8K8ICZCT78&dib=eyJ2IjoiMSJ9.GZzyj9YeMmqRwkrAxnJLvxZmp8juEAPxvWaJNDTe-BnVwPIudvqRm4sZAc7IP9eih2k2UBI28kpvyqW1UdujgM7ySWO5widZQX7z_e3htFkprf94Axis4An0fMAB4kCuZi6Zommv2BNBf8I3SemVVES1kw97-VHHl9trS3F92p_tj5bDolfV3Lu9_eWo92TuVAy9uYZJX0iSqHhAfrsuyFppO90CLPFCNlSxerc6Sdw.EZ75vNhgBS6jhn1zFlxcrBuLhYbOK52ouKRiwIzu3pM&dib_tag=se&keywords=SSD1306&qid=1752420116&sprefix=ssd1306%2Caps%2C116&sr=8-3&th=1)"> Link </a> |
| Electronics Kit | Customize the device and wires to connect components | $13.49 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6](https://www.amazon.com/Smraza-Electronics-Potentiometer-tie-Points-Breadboard/dp/B0B62RL725/ref=sxts_b2b_sx_reorder_acb_business?content-id=amzn1.sym.f63a3b0b-3a29-4a8e-8430-073528fe007f%3Aamzn1.sym.f63a3b0b-3a29-4a8e-8430-073528fe007f&crid=2IC3T44H3U3WG&cv_ct_cx=breadboard%2Bkit&dib=eyJ2IjoiMSJ9.TUd5tu2T8rmms7ZuJ0UzmbtpLL1zsu93bQM0PzwnP4E.sT0V0vL_QtbYv8ymVTCcRkhFNgBtRvRiT7G4FT1oGTE&dib_tag=se&keywords=breadboard%2Bkit&pd_rd_i=B0B62RL725&pd_rd_r=67e1f4ff-e3b9-44e4-b441-b4ae282f036b&pd_rd_w=UjFaP&pd_rd_wg=0xRoC&pf_rd_p=f63a3b0b-3a29-4a8e-8430-073528fe007f&pf_rd_r=BFGP77H27ZN31W4PZAW6&qid=1715911733&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sprefix=breadboard%2Bkit%2Caps%2C109&sr=1-2-9f062ed5-8905-4cb9-ad7c-6ce62808241a&th=1)/"> Link </a> |
| DMM | Measure the voltage and resistance | $11 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/AstroAI-Digital-Multimeter-Voltage-Tester/dp/B01ISAMUA6/ref=sxin_17_pa_sp_search_thematic_sspa?content-id=amzn1.sym.e8da13fc-7baf-46c3-926a-e7e8f63a520b%3Aamzn1.sym.e8da13fc-7baf-46c3-926a-e7e8f63a520b&cv_ct_cx=digital+multimeter&dib=eyJ2IjoiMSJ9.5LQumrfBR8l0mKnJCJlRg73dxpou0gqYD_ffU3srgs0Utegwth8GcQCSVXVzeZeLSJx5J3itz5TLdmJHsrVITQ.-00jRPoT-bBy26YC4LzQ-S4cYdztgmSMGb83_WEm6HY&dib_tag=se&keywords=digital+multimeter&pd_rd_i=B01ISAMUA6&pd_rd_r=e1ff2570-7e4a-4906-bc55-6f819d48d1bc&pd_rd_w=h7HgL&pd_rd_wg=0ZcFH&pf_rd_p=e8da13fc-7baf-46c3-926a-e7e8f63a520b&pf_rd_r=R6YKX3NXTDQ1PQP4H8RM&qid=1715911879&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sr=1-1-7efdef4d-9875-47e1-927f-8c2c1c47ed49-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9zZWFyY2hfdGhlbWF0aWM&psc=1)"> Link </a> |
| USB-USBC | Adapter | $2.99 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/ENVEL-Transfer-Converter-Thunderbolt3-Compatible/dp/B0D3T2QDVJ/ref=sxin_17_pa_sp_search_thematic_sspa?content-id=amzn1.sym.70fcaece-2dd2-4653-bf00-fb6af1af1b93%3Aamzn1.sym.70fcaece-2dd2-4653-bf00-fb6af1af1b93&crid=2XKXL9JJ62FRH&cv_ct_cx=usba%2Bto%2Busbc&keywords=usba%2Bto%2Busbc&pd_rd_i=B0D3T2QDVJ&pd_rd_r=4d705a77-7d1c-4543-b61d-c95f071f99c3&pd_rd_w=zydwI&pd_rd_wg=Ra4PI&pf_rd_p=70fcaece-2dd2-4653-bf00-fb6af1af1b93&pf_rd_r=GA7XX674ZRQ1VKTH3HWX&qid=1750358432&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sprefix=usba%2Bto%2Busb%2Caps%2C106&sr=1-1-e169343e-09af-4d41-85b1-8335fe8f32d0-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9zZWFyY2hfdGhlbWF0aWM&th=1)"> Link </a> |

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
