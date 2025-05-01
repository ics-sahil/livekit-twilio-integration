

### This is documentation for the twilio sip trunking set up, below mentioned steps will be taken in order to achieve  configuration successfully.

# For Inbound calls

## Install twilio cli

### using apt 

      wget -qO- https://twilio-cli-prod.s3.amazonaws.com/twilio_pub.asc \
        | sudo apt-key add -
      sudo touch /etc/apt/sources.list.d/twilio.list
      echo 'deb https://twilio-cli-prod.s3.amazonaws.com/apt/ /' \
        | sudo tee /etc/apt/sources.list.d/twilio.list
      sudo apt update
      sudo apt install -y twilio

## Now in order to use twilio cli
- Create an account in twilio (Skip this step in case you already have an account)
- buy a number run 
    twilio login
    twilio profiles:use <your-short-hand-profile-identifier>
    
### Create a SIP trunk using twilio cli
  #### Make sure domain name of your SIP trunk must end with (pstn.twilio.com) 

        twilio api trunking v1 trunks create \
        --friendly-name "My test trunk" \
        --domain-name "my-test-trunk.pstn.twilio.com"

### After running above command you will recieve <TWILIO-TRUNK-ID> , Copy your  <TWILIO-TRUNK-ID> and save it

## Now you have to configure your trunk for inbound calls:

#### Configure an origination URI aka <you SIP host>
Go to liveKit. Create an account in case you dont have one.
Navigate to Setting and Copy you <SIP-URI>

    twilio api trunking v1 trunks origination-urls create \
        --trunk-sid <TWILIO-TRUNK-ID> \
        --friendly-name "LiveKit SIP URI" \
        --sip-url <SIP-URI> \
        --weight 1 --priority 1 --enabled

## Associate phone Number and trunk
#### For this you need <Twilio-Trunk-SID> and <Twilio-Phone-Number-SID> If you have them saved than you can use them else you can get them  by following ways. 
  - To list phone numbers:
 
        twilio phone-numbers list
    
  - To list trunks:

        twilio api trunking v1 trunks list
    

##### Then to associate both
    twilio api trunking v1 trunks phone-numbers create \
    --trunk-sid <twilio_trunk_sid> \
    --phone-number-sid <twilio_phone_number_sid>

#### OR Configure a SIP trunk using the TWILIO UI
- Search Elastic SIP OR 
- Select Elastic SIP Trunking >> Manage >> Trunks.
- Create SIP trunk.
- For Inbound SIP
  -- Navigate to Voice >> Manage >> Origination connection policy.
- Select created origination policy.
- Add the <SIP-URI>, weight, and priority as was done earlier during trunk configuration via the CLI.

#### livekit set up
   Install livekit cli

#### Linux :
    curl -sSL https://get.livekit.io/cli | bash

#### optionally authentication with cloud
- This is in order to avoid adding api-key and api-secret in each request manually

        lk cloud auth

- You'll be prompted to set the current project as default, select Yes. This can be changed later by editing the config.yaml file (the path to which will be logged after running the above commands)
