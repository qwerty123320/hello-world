# hello-world
E-commerce platform
public class MainApplication {
    public static void main(String[] args) {
        System.out.println("🎯 欢迎使用电子商务平台");

        // 初始化模块
        UserManager_Ai.init();
        ProductManager_Lin.init();
        OrderManager_Pei.init();
        PaymentManager_Yu.init();

        // 模拟用户注册和登录
        UserManager_Ai.register("alice", "123456");
        UserManager_Ai.login("alice", "123456");

        // 模拟商品浏览
        ProductManager_Lin.listProducts();

        // 模拟下单流程
        OrderManager_Pei.createOrder("P001", 2, "alice");

        // 模拟支付流程
        PaymentManager_Yu.pay("ORDER123", 199.99);
    }
}
