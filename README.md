# hello-world
E-commerce platform
public class OrderManager_Pei {
    public static void init() {
        System.out.println("🧾 订单模块已加载");
    }

    public static void createOrder(String productId, int quantity, String username) {
        String orderId = "ORDER123"; // 模拟生成订单号
        double price = 99.99; // 假设商品价格
        double total = price * quantity;

        System.out.println("用户 " + username + " 下单：");
        System.out.println("- 商品ID：" + productId);
        System.out.println("- 数量：" + quantity);
        System.out.println("- 总价：" + total);
        System.out.println("- 生成订单号：" + orderId);
    }
}
