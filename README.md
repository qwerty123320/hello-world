# hello-world
E-commerce platform
import java.util.ArrayList;
import java.util.Date;
import java.util.List;
import java.util.UUID;

public class OrderManager_zcy {

    // 模拟订单状态
    public enum OrderStatus {
        PENDING,
        PAID,
        CANCELED
    }

    // 模拟订单对象
    public static class Order {
        private String orderId;
        private String username;
        private String productId;
        private int quantity;
        private double pricePerUnit;
        private Date createdAt;
        private OrderStatus status;

        public Order(String username, String productId, int quantity, double pricePerUnit) {
            this.orderId = generateOrderId();
            this.username = username;
            this.productId = productId;
            this.quantity = quantity;
            this.pricePerUnit = pricePerUnit;
            this.createdAt = new Date();
            this.status = OrderStatus.PENDING;
        }

        private String generateOrderId() {
            return "ORD-" + UUID.randomUUID().toString().substring(0, 8).toUpperCase();
        }

        public double getTotalPrice() {
            return pricePerUnit * quantity;
        }

        public String getOrderId() {
            return orderId;
        }

        public OrderStatus getStatus() {
            return status;
        }

        public void setStatus(OrderStatus status) {
            this.status = status;
        }

        public String toString() {
            return String.format(
                "[订单ID: %s, 用户: %s, 商品ID: %s, 数量: %d, 单价: %.2f, 总价: %.2f, 创建时间: %s, 状态: %s]",
                orderId, username, productId, quantity, pricePerUnit, getTotalPrice(), createdAt, status
            );
        }
    }

    // 存储订单的列表
    private static List<Order> orderList = new ArrayList<>();

    // 初始化模块
    public static void init() {
        System.out.println("🧾 订单模块已初始化");
    }

    // 创建订单
    public static Order createOrder(String username, String productId, int quantity, double pricePerUnit) {
        Order order = new Order(username, productId, quantity, pricePerUnit);
        orderList.add(order);
        System.out.println("✅ 成功创建订单：" + order.getOrderId());
        return order;
    }

    // 查询所有订单
    public static void listAllOrders() {
        if (orderList.isEmpty()) {
            System.out.println("⚠️ 当前没有订单记录");
            return;
        }

        System.out.println("📦 所有订单如下：");
        for (Order order : orderList) {
            System.out.println(order);
        }
    }

    // 查询某个用户的订单
    public static void listOrdersByUser(String username) {
        System.out.println("🔍 用户 " + username + " 的订单：");
        for (Order order : orderList) {
            if (order.username.equals(username)) {
                System.out.println(order);
            }
        }
    }

    // 根据订单ID查找订单
    public static Order getOrderById(String orderId) {
        for (Order order : orderList) {
            if (order.getOrderId().equals(orderId)) {
                return order;
            }
        }
        return null;
    }

    // 模拟支付订单
    public static void payOrder(String orderId) {
        Order order = getOrderById(orderId);
        if (order == null) {
            System.out.println("❌ 订单不存在：" + orderId);
            return;
        }
        if (order.getStatus() != OrderStatus.PENDING) {
            System.out.println("⚠️ 订单无法支付，当前状态：" + order.getStatus());
            return;
        }
        order.setStatus(OrderStatus.PAID);
        System.out.println("💰 支付成功，订单已完成：" + order.getOrderId());
    }

    // 取消订单
    public static void cancelOrder(String orderId) {
        Order order = getOrderById(orderId);
        if (order == null) {
            System.out.println("❌ 无法取消，订单不存在：" + orderId);
            return;
        }
        if (order.getStatus() == OrderStatus.CANCELED) {
            System.out.println("⚠️ 订单已被取消：" + orderId);
            return;
        }
        if (order.getStatus() == OrderStatus.PAID) {
            System.out.println("⚠️ 订单已支付，无法取消：" + orderId);
            return;
        }
        order.setStatus(OrderStatus.CANCELED);
        System.out.println("❎ 订单已取消：" + order.getOrderId());
    }

    // 示例测试
    public static void main(String[] args) {
        init();
        Order order1 = createOrder("alice", "P001", 2, 299.99);
        Order order2 = createOrder("bob", "P002", 1, 1599.00);
        Order order3 = createOrder("alice", "P003", 3, 39.90);

        listAllOrders();
        System.out.println();

        listOrdersByUser("alice");
        System.out.println();

        payOrder(order1.getOrderId());
        cancelOrder(order2.getOrderId());
        cancelOrder(order1.getOrderId()); // 尝试取消已支付订单

        System.out.println("\n🧾 订单状态更新后：");
        listAllOrders();
    }
}
import java.util.ArrayList;
import java.util.List;
import java.util.UUID;

public class ProductManager_Lin {

    // 商品分类枚举
    public enum Category {
        ELECTRONICS, CLOTHING, BOOKS, TOYS, HOME
    }

    // 商品实体类
    public static class Product {
        private String productId;
        private String name;
        private Category category;
        private double price;
        private int stock;

        public Product(String name, Category category, double price, int stock) {
            this.productId = generateProductId();
            this.name = name;
            this.category = category;
            this.price = price;
            this.stock = stock;
        }

        private String generateProductId() {
            return "P-" + UUID.randomUUID().toString().substring(0, 8).toUpperCase();
        }

        public String getProductId() {
            return productId;
        }

        public String getName() {
            return name;
        }

        public Category getCategory() {
            return category;
        }

        public double getPrice() {
            return price;
        }

        public int getStock() {
            return stock;
        }

        public void setPrice(double price) {
            this.price = price;
        }

        public void setStock(int stock) {
            this.stock = stock;
        }

        public String toString() {
            return String.format("[ID: %s, 名称: %s, 分类: %s, 价格: %.2f, 库存: %d]",
                    productId, name, category, price, stock);
        }
    }

    // 商品列表（模拟数据库）
    private static List<Product> productList = new ArrayList<>();

    // 初始化模块
    public static void init() {
        System.out.println("🛒 商品模块已初始化");

        // 加载一些默认商品
        addProduct("小米13", Category.ELECTRONICS, 3999.00, 50);
        addProduct("T恤", Category.CLOTHING, 129.00, 200);
        addProduct("Java 编程思想", Category.BOOKS, 89.90, 30);
        addProduct("遥控小汽车", Category.TOYS, 59.90, 100);
        addProduct("电饭煲", Category.HOME, 299.00, 20);
    }

    // 添加商品
    public static Product addProduct(String name, Category category, double price, int stock) {
        Product product = new Product(name, category, price, stock);
        productList.add(product);
        System.out.println("✅ 添加商品成功：" + product);
        return product;
    }

    // 获取所有商品
    public static void listAllProducts() {
        if (productList.isEmpty()) {
            System.out.println("⚠️ 没有商品可展示");
            return;
        }
        System.out.println("📦 所有商品列表：");
        for (Product p : productList) {
            System.out.println(p);
        }
    }

    // 根据分类查找商品
    public static void listProductsByCategory(Category category) {
        System.out.println("🔍 分类：" + category + " 的商品：");
        for (Product p : productList) {
            if (p.getCategory() == category) {
                System.out.println(p);
            }
        }
    }

    // 根据ID查找商品
    public static Product getProductById(String productId) {
        for (Product p : productList) {
            if (p.getProductId().equals(productId)) {
                return p;
            }
        }
        return null;
    }

    // 修改价格
    public static void updatePrice(String productId, double newPrice) {
        Product p = getProductById(productId);
        if (p == null) {
            System.out.println("❌ 商品不存在：" + productId);
            return;
        }
        p.setPrice(newPrice);
        System.out.println("✅ 更新价格成功：" + p);
    }

    // 修改库存
    public static void updateStock(String productId, int newStock) {
        Product p = getProductById(productId);
        if (p == null) {
            System.out.println("❌ 商品不存在：" + productId);
            return;
        }
        p.setStock(newStock);
        System.out.println("✅ 更新库存成功：" + p);
    }

    // 删除商品
    public static void deleteProduct(String productId) {
        Product p = getProductById(productId);
        if (p == null) {
            System.out.println("❌ 无法删除，商品不存在：" + productId);
            return;
        }
        productList.remove(p);
        System.out.println("🗑️ 商品已删除：" + productId);
    }

    // 示例测试
    public static void main(String[] args) {
        init();

        System.out.println();
        listAllProducts();

        System.out.println();
        listProductsByCategory(Category.ELECTRONICS);

        System.out.println();
        Product test = addProduct("手工笔记本", Category.BOOKS, 35.00, 10);
        updatePrice(test.getProductId(), 29.99);
        updateStock(test.getProductId(), 50);

        System.out.println();
        deleteProduct(test.getProductId());

        System.out.println();
        listAllProducts();
    }
}


