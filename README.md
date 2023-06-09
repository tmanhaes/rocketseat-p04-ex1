# Entities

### Product

Representa o produto sem especificações para efeitos de agrupamentos futuros independentes de SKUs.

### Product SKU

Representa um SKU de produto, ou seja, uma variação de produto de cor e tamanho específico.

##### Attributes:

- product
- color?
- size?
- minInventory

### InventoryMovimentation

Representam as entradas e saídas de estoque para cada sku de produto, permitindo apuração da posição do estoque para cada produto.

### InventoryPurchaseOrder

Representam as entradas de estoque para cada sku de produto, permitindo apuração da posição do estoque para cada produto quando comparada as saídas em Vendas.

Pode ter um código identificador do lote para ser possível apuração financeira do estoque para efeitos de balanço patrimonial (montante investido em estoque)

### Inventory

Representa a posicão atual (quantidade) de cada item em estoque.

Pode ser apurado financeiramente com métodos de LIFO ou LIFO. Caso haja um código identificador para cada ordem de compra, é possível apuração financeira precisa da entrada e saída de cada item (ou lote de compra).

### Order

Representa as ordens de venda.

### SystemNotification

Representa a notificação dentro do sistema com uma message, um context (por exemplo, inventory purchase) e um status (lida ou não lida).

# UseCases (Ações)

#### createProduct

#### createProductSKU

#### createInventoryPurchaseOrder

#### createOrder

#### updateProduct

#### updateProductSKU

#### updateInventoryPurchase

#### updateOrder

#### removeProduct

#### removeProductSKU

#### cancelInventoryPurchaseOrder

#### cancelOrder

#### defineMinInvetoryForProduct

- Input: ProductSKU Id
- Output: void

#### getSalesByPeriod

- Input: start, end
- Output: {date: { productSKUId, quantity }[] }

#### getSalesByProductSKU

- Input: ProductSKU Id
- Output: { productSKUId: quantity, salesValue }

#### getSalesByProduct

- Input: Product Id
- Output: { productSkuId: quantity, salesValue }[]

#### getInventoryPositionByProductSKU

- Input: ProductSKU Id
- Output: { productSKUId, inventoryPosition}

#### getInventoryHistoryByPeriod

- Input: start, end
- Output: { date:
  {productSKUId:
  initiaInventoryPosition,
  finalInventoryPositionAtDate,
  purchases: inventoryPurchase[],
  sales: order[]
  }[]
  }[]

#### listInventory

- Input: ordenation (optional)
- Output

#### sendNotification (ou registerSystemNotification)

- Input: message, context (ou mesmo SystemNotification)
- Output: void

# Domain Event

#### Notification (Low Inventory)

A cada compra, UseCase valida pela entidade Inventory se posição está abaixo do mínimo (inventory position contra Product SKU.minInventory) e resultado pode gerar um evento com dois handles:

- enviar notificação para o usuário responsável.
- enviar e-mail para o usuário responsável.
