# PMoC

add text here

A map by the people of the people for the people.

The purpose of this App is deisgned to let the people of Corvallis and the world share and unite their visions and representations of the world.

To start this app, please upload Android software the internet is hungry.

To do list:

*Add Android to Github

*Add GoogleMaps API

*Add Functionality to Upload images

*Add Functionality to Resize images

*Add Functionality to pin images to Map

Add AndroidStudioProjects\APeoplesMapofCorvallisApp\app\build\intermediates



-----------------------------------------------------

First Screen:
Welcome Screen
Button"Contribute" - Button
Button"Just Look"
Button"About"

----------------------------------------------------

Contribute
"What would you like to contribute?"
Button"Audio"
Button"Video"
Button"Picture"

----------------------------------------------------

Just Look
""
""

----------------------------------------------------

About
""
""

----------------------------------------------------

Picture Screen
"Explaination text"
ButtonE"Upload"
ButtonE"Camera"
ButtonL"Last Photo"
ButtonL"Editing Tools"


----------------------------------------------------

Camera
Intent intent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE); 
    captured_image = System.currentTimeMillis() + ".jpg";
    File file = new File(Environment.getExternalStorageDirectory(), captured_image); 
    captured_image = file.getAbsolutePath();
    Uri outputFileUri = Uri.fromFile(file); 
    intent.putExtra(MediaStore.EXTRA_OUTPUT, outputFileUri); 
    intent.putExtra("return-data", true);
    ((Activity) GlobalVars.main_ctx).startActivityForResult(intent, RES_IMAGE_CAPTURE);
    
    Then you need a ActivityResulListener Like:

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data)
{
    switch (requestCode) { 
        case RES_IMAGE_CAPTURE: 

            Log.i( "MakeMachine", "resultCode: " + resultCode );
            switch( resultCode )
            {
                case 0:
                    Log.i( "MakeMachine", "User cancelled" );
                    break;
                case -1:
                    //image storead, now load it in the web
                    break;
                }
            break;

    }   
}
After storing the Picture you have to perform a Post Request to load the picture in the web, you need script wich is copying the file to the server, maybe asp.net and than you only have to perform the Request. I only have a code for https Requests with credentials, using a External Libary from appache, this might be a little bit too complicated, but I'm sure you will finde a code here, otherwise my solution looks like:

public static boolean upload_image(String url, List<NameValuePair> nameValuePairs,String encoding) {

    DefaultHttpClient http = new DefaultHttpClient();
        SSLSocketFactory ssl =  (SSLSocketFactory)http.getConnectionManager().getSchemeRegistry().getScheme( "https" ).getSocketFactory(); 
        ssl.setHostnameVerifier( SSLSocketFactory.ALLOW_ALL_HOSTNAME_VERIFIER );
        final String username = "username";
        final String password = "password";
        UsernamePasswordCredentials c = new UsernamePasswordCredentials(username,password);
        BasicCredentialsProvider cP = new BasicCredentialsProvider(); 
        cP.setCredentials(AuthScope.ANY, c); 
        http.setCredentialsProvider(cP);
        HttpResponse res;
        try {
            HttpPost httpost = new HttpPost(url);
            MultipartEntity entity = new MultipartEntity(HttpMultipartMode.STRICT); 

            for(int index=0; index < nameValuePairs.size(); index++) { 
                ContentBody cb;
                if(nameValuePairs.get(index).getName().equalsIgnoreCase("File")) { 
                    File file = new File(nameValuePairs.get(index).getValue());
                    FileBody isb = new FileBody(file,"application/*");
                    entity.addPart(nameValuePairs.get(index).getName(), isb);
                } else { 
                    // Normal string data 
                    cb =  new StringBody(nameValuePairs.get(index).getValue(),"", null);
                    entity.addPart(nameValuePairs.get(index).getName(),cb); 
                } 
            } 


            httpost.setEntity(entity);
            res = http.execute(httpost);

            InputStream is = res.getEntity().getContent();
            BufferedInputStream bis = new BufferedInputStream(is);
            ByteArrayBuffer baf = new ByteArrayBuffer(50);
            int current = 0;
            while((current = bis.read()) != -1){
                  baf.append((byte)current);
             }
            res = null;
            httpost = null;
            String ret = new String(baf.toByteArray(),encoding);
            GlobalVars.LastError = ret;
            return  true;
           } 
        catch (ClientProtocolException e) {
            // TODO Auto-generated catch block
            return true;
        } 
        catch (IOException e) {
            // TODO Auto-generated catch block
            return true;
        } 

} 
""
""

----------------------------------------------------
