# Snippet-PDO-con-commit-PHP

```php

$sql = "SET @update_id := '';
        UPDATE testing SET status='1', id=(SELECT @update_id:=id)
        WHERE status='0' LIMIT 1";
try{
    $db->beginTransaction();
    $db->query($sql); // no need for prepare/execute since there are no parameters
    $stmt = $db->query("SELECT @update_id");
    $row = $stmt->fetch(PDO::FETCH_ASSOC);
    $id = $row['@update_id'];
    $db->commit();
} catch (Exception $e) {
    echo $e->getMessage();
    $db->rollBack();
    exit();
}
```
