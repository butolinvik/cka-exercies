# Create chart
helm create mychart
# Создадутся папки с набором файлов
---Templates----
# Проверка отработки шаблонов
helm install --dry-run --debug ./vicchat --generate-name

# Обновление пакета
```bash
helm upgrade vmagent ./vmagent/  

```