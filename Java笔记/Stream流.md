## JAVA常用String inputstream转换

1. String –> InputStream

   ```java
   InputStream is = new ByteArrayInputStream(str.getBytes());
   或者
   ByteArrayInputStream stream= new ByteArrayInputStream(str.getBytes());
   ```

2. InputStream–>String

   ```java
   inputStream input;

   StringBuffer out = new StringBuffer();
        byte[] b = new byte[4096];
        for (int n; (n = input.read(b)) != -1;) {
             out.append(new String(b, 0, n));
        }
   out.toString();
   ```

3. Reader –>String

   ```java
   BufferedReader in = new BufferedReader(new InputStreamReader(is));
   StringBuffer buffer = new StringBuffer();
   String line = " ";
   while ((line = in.readLine()) != null){
        buffer.append(line);
   }
   return buffer.toString();
   ```

4. String–>Reader

   ```java
   Reader reader = null;
   BufferedReader r = new BufferedReader(reader);
   StringBuilder b = new StringBuilder();
   String line;
   while((line=r.readLine())!=null) {
        b.append(line);
        b.append(“\r\n”);
   }
   b.toString();
   ```

   ​

