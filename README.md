

# this is documentation for the twilio sip trunking set up , below mentioned steps will be taken 
# in order to achieve  configuration successfully.

# For Inbound calls

1 install twilio cli:: 

# using apt 


wget -qO- https://twilio-cli-prod.s3.amazonaws.com/twilio_pub.asc \
  | sudo apt-key add -
sudo touch /etc/apt/sources.list.d/twilio.list
echo 'deb https://twilio-cli-prod.s3.amazonaws.com/apt/ /' \
  | sudo tee /etc/apt/sources.list.d/twilio.list
sudo apt update
sudo apt install -y twilio


# Now in order to use twilio cli ..
    create account in twilio (Skip this step in case you already have an account)
    ->  buy a number .
    Now.

    run `twilio login`

    twilio profiles:use <your-short-hand-profile-identifier> 

        

# Create a sip trunk using twilio cli

    ### Make sure domain name of your SIP trunk must end with (pstn.twilio.com)

    for example  
         Here we are making  My test trunk 
         and with domian my-test-trunk.pstn.twilio.com   

        twilio api trunking v1 trunks create \
        --friendly-name "My test trunk" \
        --domain-name "my-test-trunk.pstn.twilio.com"

      You can name trunk as you feel comfortable with.

# After running above command you will recieve <TWILIO-TRUNK-ID> , Copy your  <TWILIO-TRUNK-ID> and save it


# Now you have to configure your above created trunk for inbound calls:

    1. Configure an origination URI aka <you SIP host>

    Go to liveKit. Register in case you are not .
     ->> Setting 
        Copy you <SIP-URI>



    twilio api trunking v1 trunks origination-urls create \
        --trunk-sid <TWILIO-TRUNK-ID> \
        --friendly-name "LiveKit SIP URI" \
        --sip-url <SIP-URI> \
        --weight 1 --priority 1 --enabled

    

# Associate phone Number and trunk

    For this you need <Twilio-Trunk-SID> and <Twilio-Phone-Number-SID>

    If you have them saved than you can use them else you can get them 

    by following ways.

    To list phone numbers: twilio phone-numbers list
    To list trunks: twilio api trunking v1 trunks list

    twilio api trunking v1 trunks phone-numbers create \
    --trunk-sid <twilio_trunk_sid> \
    --phone-number-sid <twilio_phone_number_sid>




    # OR Configure a SIP trunk using the TWILIO UI

        Search Elastic SIP OR 

        Select Elastic SIP Trunking >> Manage >> Trunks.

        Create SIP trunk.


    For Inboud SIP

        Navigate to Voice >> Manage >> Origination connection policy.

        Select created origination policy.

        Add the <SIP-URI>, weight, and priority as was done earlier during trunk configuration via the CLI.




# livekit set up

    Install livekit cli

# Linux :
        curl -sSL https://get.livekit.io/cli | bash

    optionally authentication with cloud

        This is in order to avoid adding api-key and api-secret in each request manually


        lk cloud auth

         >> ask for setting current project as default ,
            which can be later changed by editing <path-to-default-user>/.livekit/cli.config.yaml 


# Inbound Trunk setup on Livekit side:

 inbound-trunk.json file

    {
     "trunk": {
    "name": "My inbound trunk",
        "numbers": ["+$$$$$$$$$$"] # your twilio number
     }
    }



# now create 


        


        

        



    













    
      


    









