#%RAML 1.0
title: chatgpt-eapi
description: Asyncronous API for pushing notifications out. Notifications are consumed by Anypint MQ

types:
 Notification:
   type: object
   properties:
     id?:
       type: string
     priority:
       enum:
         - High
         - Standard
         - Log
     groupId: string | number
     message: string
     status?:
       enum:
         - pending
         - complete
         - failed
        
/notification:
 get:
   description: Returns a list of notifications
   queryParameters:
     status:
       description: Filter by status
       type: string
       required: false
   responses:
     200:
       body:
         application/json:
           type: Notification[]
           example:
             -
               id: 5f7486a192ea33fe3e2f08b9
               status: pending
               message: This is a sample notification
               priority: High
               groupId: 1
             -
               id: 5f74939b92ea33fe3e2f08c6
               status: complete
               message: This is a sample notification
               priority: Log
               groupId: 0
 post:
   description: Create a new noitification
   body:
     application/json:
       type: Notification
       example:
         message: This is a test notification
         priority: Standard
         groupId: 1
   responses:
     204:
       description: Notification has been created
       body:
         application/json:
           example: {
             "id": "5f7486a192ea33fe3e2f08b9",
             "status": "pending"
           }
 /{id}:
   get:
     description: Returns the status of the notification
     queryParameters:
       details:
         description: By default, only the status of the notification is returned. For complete information, include ?details=true
         type: boolean
         required: false
         default: false
     responses:
       200:
         body:
           application/json:
             example: { "status": "pending" }
       404:
         description: Notification does not exist