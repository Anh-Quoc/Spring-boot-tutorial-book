# Paging

<mark style="background-color:red;">!!!</mark>  <mark style="color:red;">**THIS SOLUTION IS NOT COMPLETE BECAUSE WE DON'T HAVE THE NUMBER OF PAGE**</mark>

{% code lineNumbers="true" %}
```sql
    CREATE PROC usp_ProductFindByPage(@page_index int, @page_size int)
    AS
    BEGIN
          WITH product_index
          AS (
              SELECT P.*, ROW_NUMBER() OVER (ORDER BY ProductID ASC) AS [index]
              FROM [dbo].[Products] p
          )
          SELECT *
          FROM product_index ps
          WHERE ps.[index] BETWEEN (@page_index - 1) * @page_size + 1 AND @page_index * @page_size
    END
```
{% endcode %}

```java
    public List<Product> findByPage(int pageIndex){
        List<Product> listProduct;
        try(Connection connection = DBContext.getConnection()){
            PreparedStatement statement = connection.prepareStatement("{CALL usp_ProductFindByPage(?, ?)}");
            statement.setInt(1, pageIndex);
            statement.setInt(2, Constants.PAGE_SIZE);
            ResultSet resultSet = statement.executeQuery();

            listProduct = new ArrayList<>();
            Product product;
            while (resultSet.next()){
                product = new Product();
                product.setProductID(resultSet.getInt("ProductID"));
                product.setProductName(resultSet.getString("ProductName"));
                product.setSupplierID(resultSet.getInt("SupplierID"));
                product.setColor(resultSet.getString("Color"));
                product.setPackageType(resultSet.getString("PackageType"));
                product.setSize(resultSet.getString("Size"));
                product.setTaxRate(resultSet.getDouble("TaxRate"));
                product.setUnitPrice(resultSet.getDouble("UnitPrice"));
                product.setRecommendedRetailPrice(resultSet.getDouble("RecommendedRetailPrice"));
                product.setTypicalWeightPerUnit(resultSet.getDouble("TypicalWeightPerUnit"));

                listProduct.add(product);
            }

        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
        return listProduct;
    }

```
