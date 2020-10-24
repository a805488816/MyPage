 
 #### 如何用java获取页面
```java
@Controller
@RequestMapping("/services/customized/public/rs/")
public class MiniMainController {
    private int port;
    private String host;
    private Socket socket;
    private BufferedReader bufferedReader;
    private BufferedWriter bufferedWriter;

    /**
     * 跳转到添加页面
     */
    @PostMapping("/weixinMiniAppPublicService/login")
    public String login(JSONObject json) {
        System.out.println(json);
        return "/system/user/add";
    }

    public boolean SocketForHttpTest(String host, int port) throws IOException {

        this.host = host;
        this.port = port;

        /**
         * http协议
         */
         socket = new Socket(this.host, this.port);

        /**
         * https协议
         */
//        socket = (SSLSocket)((SSLSocketFactory)SSLSocketFactory.getDefault()).createSocket(this.host, this.port);

        return true;
    }

    public void sendGet() throws IOException {
        //String requestUrlPath = "/z69183787/article/details/17580325";
        String requestUrlPath = "/house-service/services/customized/public/rs/weixinMiniAppPublicService/login";

        OutputStreamWriter streamWriter = new OutputStreamWriter(socket.getOutputStream());
        bufferedWriter = new BufferedWriter(streamWriter);
        bufferedWriter.write("GET " + requestUrlPath + " HTTP/1.1\r\n");
        bufferedWriter.write("Host: " + this.host + "\r\n");
        bufferedWriter.write("\r\n");
        bufferedWriter.flush();

        BufferedInputStream streamReader = new BufferedInputStream(socket.getInputStream());
        bufferedReader = new BufferedReader(new InputStreamReader(streamReader, "utf-8"));
//        返回值输出
        String line = null;
        while((line = bufferedReader.readLine())!= null){
            System.out.println(line);
        }
        bufferedReader.close();
        bufferedWriter.close();
        socket.close();

    }

    public void sendPost() throws IOException {
        String requestUrlPath = "/house-service/services/customized/public/rs/weixinMiniAppPublicService/login";
        String data = URLEncoder.encode("name", "utf-8") + "=" + URLEncoder.encode("张三", "utf-8") + "&" +
                URLEncoder.encode("age", "utf-8") + "=" + URLEncoder.encode("32", "utf-8");
        // String data = "name=zhigang_jia";
        System.out.println(">>>>>>>>>>>>>>>>>>>>>"+data);
        OutputStreamWriter streamWriter = new OutputStreamWriter(socket.getOutputStream(), "utf-8");
        bufferedWriter = new BufferedWriter(streamWriter);
        bufferedWriter.write("POST " + requestUrlPath + " HTTP/1.1\r\n");
        bufferedWriter.write("Host: " + this.host + "\r\n");
        bufferedWriter.write("Content-Length: " + data.length() + "\r\n");
        bufferedWriter.write("Content-Type: application/x-www-form-urlencoded\r\n");
        bufferedWriter.write("\r\n");
        bufferedWriter.write(data);

        bufferedWriter.write("\r\n");
        bufferedWriter.flush();

        BufferedInputStream streamReader = new BufferedInputStream(socket.getInputStream());
        bufferedReader = new BufferedReader(new InputStreamReader(streamReader, "utf-8"));
        String line = null;
        while((line = bufferedReader.readLine())!= null)
        {
            System.out.println(line);
        }
        bufferedReader.close();
        bufferedWriter.close();
        socket.close();
    }
    
    //执行
    public String send(Model model) throws IOException {
        MiniMainController min = new MiniMainController();
        min.SocketForHttpTest("127.0.0.1", 8082);
        min.sendPost();
        return "";
    }
}
```
#### 另一个类型的get——post
```java
public class MiniMainController {
    public String sendGet(String url, String param) throws IOException {
        String result = "";
        BufferedReader in = null;
        try {
            String urlNameString = url + "?" + param;
            URL realUrl = new URL(urlNameString);
            // 打开和URL之间的连接
            URLConnection connection = realUrl.openConnection();
            // 设置通用的请求属性
            connection.setRequestProperty("accept", "*/*");
            connection.setRequestProperty("connection", "Keep-Alive");
            connection.setRequestProperty("user-agent",
                    "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1;SV1)");
            // 建立实际的连接
            connection.connect();
            // 获取所有响应头字段
            Map<String, List<String>> map = connection.getHeaderFields();
            // 遍历所有的响应头字段
            for (String key : map.keySet()) {
                System.out.println(key + "--->" + map.get(key));
            }
            // 定义 BufferedReader输入流来读取URL的响应
            in = new BufferedReader(new InputStreamReader(
                    connection.getInputStream()));
            String line;
            while ((line = in.readLine()) != null) {
                result += line;
            }
        } catch (Exception e) {
            System.out.println("发送GET请求出现异常！" + e);
            e.printStackTrace();
        }
        // 使用finally块来关闭输入流
        finally {
            try {
                if (in != null) {
                    in.close();
                }
            } catch (Exception e2) {
                e2.printStackTrace();
            }
        }
        return result;

    }

    public String sendPost(String url,String param) throws IOException {
        PrintWriter out = null;
        BufferedReader in = null;
        String result = "";
        try {
            URL realUrl = new URL(url);
            // 打开和URL之间的连接
            URLConnection conn = realUrl.openConnection();
            // 设置通用的请求属性
            conn.setRequestProperty("accept", "*/*");
            conn.setRequestProperty("connection", "Keep-Alive");
            conn.setRequestProperty("user-agent",
                    "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1;SV1)");
            // 发送POST请求必须设置如下两行
            conn.setDoOutput(true);
            conn.setDoInput(true);
            // 获取URLConnection对象对应的输出流
            out = new PrintWriter(conn.getOutputStream());
            // 发送请求参数
            out.print(param);
            // flush输出流的缓冲
            out.flush();
            // 定义BufferedReader输入流来读取URL的响应
            in = new BufferedReader(
                    new InputStreamReader(conn.getInputStream()));
            String line;
            while ((line = in.readLine()) != null) {
                result += line;
            }
        } catch (Exception e) {
            System.out.println("发送 POST 请求出现异常！" + e);
            e.printStackTrace();
        }
        // 使用finally块来关闭输出流、输入流
        finally {
            try {
                if (out != null) {
                    out.close();
                }
                if (in != null) {
                    in.close();
                }
            } catch (IOException ex) {
                ex.printStackTrace();
            }
        }
        return result;
    }
}
```