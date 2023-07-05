# IMPACT-DAITA-Matomo
An how-to guide to deploy Matomo web analytics to inform and improve clinical trial recruitment in a privacy preserving way. This project is funded by American Heart Association (AHA).

## Description

Matomo is the leading Free/Libre open analytics platform.

Matomo is a full-featured PHP MySQL software program that you download and install on your own webserver.
At the end of the five-minute installation process, you will be given a JavaScript code.
Simply copy and paste this tag on websites you wish to track and access your analytics reports in real-time.

Matomo aims to be a free and open-source alternative to Google Analytics and is already used on more than 1,400,000 websites. Privacy is built-in!

## Install Matomo

  - [Download Matomo](https://matomo.org/download/)
  - Upload matomo to the UVM WebDB webserver
  - Point your browser to the directory
  - Follow the steps
  - Add the given javascript code to your pages to track

See https://matomo.org/docs/installation/.

When using Matomo for development you need to [install Matomo from the Git repository](https://matomo.org/faq/how-to-install/faq_18271/).

## Getting Started with WebDB

  - [Create Datases in UVM WebDB Server](https://webdb.uvm.edu/account/)
  - Generate new Passwords for the Webserver accounts
  - [Check the details of Databases in phpMyAdmin](https://webdb.uvm.edu/phpMyAdmin/)
  
## Getting Started with Silk Server

  - [Use this Manual to connect your terminal to Silker server](https://silk.uvm.edu/manual/)
  - Use `ssh -oHostKeyAlgorithms=+ssh-dss myusername@w3.uvm.edu` if you are unable to connect to your silk server.
  - Use your password to connect.

## Getting Started with Matomo Wizard Setup

  - Once you have successfully connected to your Silk server through the terminal, navigate to the `www-root` folder using the `cd` command.
  - Clone the Matomo GitHub repository by running the following command in the terminal: `git clone https://github.com/matomo-org/matomo.git`
  - Once the repository is cloned, navigate to the matomo folder using the `cd` command.
  - Once the cloning is complete, you can access your Matomo Wizard Setup by visiting `http://myusername@w3.uvm.edu/matomo` in your web browser.
  - Follow the instructions in the wizard to set up your Matomo installation. 
  - You will be prompted to provide information such as the database details (Database: `WebDB`), admin login credentials, and website information.

## Integrating Matomo with Weebly Website

  1. **Set up Matomo**: Follow the instructions provided in the "Getting Started with Silk Server" and "Getting Started with Matomo Wizard Setup" sections above to install and configure Matomo.

  2. **Obtain the tracking code**: After completing the Matomo setup, you'll receive a JavaScript tracking code. You'll need to add this code to your Weebly website to enable tracking.

  3. **Log in to your Weebly account**: Access your Weebly account and navigate to the website you want to track with Matomo.

  4. **Add the tracking code to your Weebly site**:

    - Click on the "Settings" tab in the Weebly editor.
    - Choose the "SEO" option from the left sidebar.
    - Scroll down to the "Header Code" section.
    - Paste your Matomo `JavaScript tracking code` into the "Header Code" field.
    - Click the "Save" button at the bottom of the page to save your changes.
    - Publish your website to make the changes live.
    
By following these steps, you'll integrate Matomo with your Weebly website and start tracking user behavior and traffic. This data will provide valuable insights into your website's performance, which can help inform future improvements and optimizations.

For more information Checkout `Resources Section` below.

## Features of Matomo

Matomo offers a wide range of features to help you better understand your website's traffic and user behavior. Some of the key features include:

  - Real-time data updates
  - Customizable dashboard
  - Visitor profiles
  - E-commerce tracking
  - Goal tracking and conversion optimization
  - Event tracking
  - Content tracking
  - Campaign tracking
  - Custom dimensions and variables
  - Geolocation
  - Log analytics
  - Privacy and GDPR compliance

## Integrating Matomo with Clinical Trial Recruitment Platforms

Integrating Matomo with clinical trial recruitment platforms can help you gain insights into user behavior and optimize the recruitment process. Here are the steps to follow:

  1. **Set up tracking**: Install Matomo and add the JavaScript tracking code to the clinical trial recruitment platform's pages.

  2. **Define goals**: Set up goals in Matomo that correspond to important actions on your platform, such as users signing up for a trial, completing a pre-screening questionnaire, or downloading trial-related documents.

  3. **Track user behavior**: Use Matomo's tracking features to monitor user behavior, such as which pages they visit, how long they spend on each page, and the path they take to complete a goal.

  4. **Analyze data**: Analyze the data collected by Matomo to identify trends, patterns, and areas for improvement. Look for high bounce rates, low conversion rates, or underperforming pages.

  5. **Optimize the platform**: Use the insights gained from Matomo to optimize the clinical trial recruitment platform. This might involve redesigning the user interface, streamlining the sign-up process, or improving the content to better engage potential trial participants.

  6. **Monitor and iterate**: Continuously monitor the platform's performance and make data-driven decisions to refine the recruitment process and improve overall efficiency.

## Privacy Considerations

As Matomo is built with privacy in mind, it is an ideal choice for clinical trial recruitment platforms that need to adhere to strict data privacy regulations. Here are some steps to ensure privacy while using Matomo:

  - Configure Matomo to anonymize IP addresses.
  - Enable the "Do Not Track" feature to respect user preferences.
  - Store data on your own server rather than relying on third-party data storage.
  - Regularly review and update your privacy policy to inform users about data collection and usage practices.
  - Consider obtaining user consent before tracking their activities on the platform.

## Resources

For more information and support, refer to the following resources:

  - [Matomo User Guides](https://matomo.org/docs/)
  - [Matomo Community Forums](https://forum.matomo.org/)
  - [Matomo Developer Documentation](https://developer.matomo.org/)
  - [Matomo FAQ](https://matomo.org/faq/)
  - [Matomo and Weebly Integration with Images](https://docs.google.com/document/d/17CsZKagDTxeocWQkYUqonn3MDvqZcW0-tVR6yrkXq1s/edit?usp=sharing)

By following this guide and to leverage Matomo's powerful analytics features, you can optimize clinical trial recruitment processes while preserving user privacy.

## Conclusion

Matomo is an excellent choice for clinical trial recruitment platforms that require comprehensive analytics features while maintaining a strong focus on privacy. By deploying Matomo, you can gain valuable insights into user behavior and optimize the recruitment process to improve overall efficiency. Moreover, you can ensure data privacy and compliance with regulations such as GDPR. Use the resources provided to make the most of Matomo and drive successful clinical trial recruitment.
