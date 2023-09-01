# Deployment

  
 "start-firebase-emulators": "firebase emulators:start",
  "stop-firebase-emulators": "pkill -f 'firebase/emulator'",
  "deploy-preview": "firebase hosting:channel:deploy preview",
  "deploy-dev": "firebase deploy --project awesomeapp-dev",
    "deploy-prod": "firebase deploy --project awesomeapp-prod"
  ```

## Hosting Sites
      
preview channel expires after 7days. changeable via --expires 1d
      live channels never expire
