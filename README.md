# Bài 3: Xử lý xung đột phức tạp trong quá trình Rebase

## 1. Mục tiêu

Thực hiện rebase nhánh `feature-api` lên nhánh `main` và xử lý xung đột thủ công tại từng commit.

Yêu cầu đảm bảo lịch sử Git thẳng, không sử dụng merge commit và không làm mất thay đổi của nhánh `main`.

## 2. Các commit đã tạo

### Nhánh main

```text
init config
update port on main
add env config
```

### Nhánh feature-api

```text
feat: change port
feat: enable debug
```

## 3. Thực hiện Rebase

Đứng trên nhánh `feature-api` và chạy:

```bash
git checkout feature-api
git rebase main
```

## 4. Conflict lần 1

Conflict xảy ra khi Git áp dụng commit:

```text
feat: change port
```

Nguyên nhân là cả `main` và `feature-api` đều thay đổi trường `port` trong file `config.json`.

Nhánh `main` có:

```json
"port": 8081
```

Trong khi `feature-api` có:

```json
"port": 9000
```

Để không làm mất thay đổi của nhánh chính, giữ giá trị:

```json
"port": 8081
```

Sau khi sửa conflict:

```bash
git add config.json
git rebase --continue
```

## 5. Conflict lần 2

Conflict tiếp tục xảy ra khi Git áp dụng commit:

```text
feat: enable debug
```

Commit này thay đổi:

```json
"debug": false
```

thành:

```json
"debug": true
```

Sau khi xử lý, giữ các thay đổi của `main` và áp dụng thay đổi `debug` của `feature-api`.

File `config.json` cuối cùng:

```json
{
  "port": 8081,
  "debug": true,
  "env": "production"
}
```

Sau đó chạy:

```bash
git add config.json
git rebase --continue
```

## 6. Kiểm tra sau khi Rebase

Kiểm tra trạng thái:

```bash
git status
```

Kết quả:

```text
On branch feature-api
nothing to commit, working tree clean
```

Kiểm tra lịch sử:

```bash
git log --graph --oneline --all
```

Lịch sử sau Rebase có dạng:

```text
* feat: enable debug
* feat: change port
* add env config
* update port on main
* init config
```

Lịch sử không có merge commit phụ.

## 7. Kết quả

Đã hoàn thành Rebase nhánh `feature-api` lên `main`.

Các commit của `feature-api` được đặt nối tiếp sau các commit mới nhất của `main`.

Các conflict được giải quyết thủ công tại từng chặng và không sử dụng merge thông thường.
