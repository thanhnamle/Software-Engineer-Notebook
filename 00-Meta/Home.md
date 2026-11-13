---
title: "Home"
type: moc
status: evergreen
tags: [moc, dashboard]
created: 2026-11-13
updated: 2026-11-13
---

# 🏠 Engineering Handbook — Home

> [!abstract] Trọng tâm hiện tại
> **C# · ASP.NET Core · Clean Architecture · CQRS + MediatR · EF Core**
> Lộ trình: [[Roadmap-DotNet]] · Bảng tiến độ: [[Learning-Board]] · Bản đồ Java→.NET: [[Java-to-DotNet-Mental-Map]]

## 🧭 Điểm vào nhanh
| Khu vực | Note gốc |
| --- | --- |
| 🟣 .NET / C# | [[Java-to-DotNet-Mental-Map]] |
| 🏛️ Kiến trúc | [[Clean-Architecture-in-DotNet]] · [[CQRS-and-MediatR-Pattern]] |
| 🧪 Thực hành | [[POC-01-Ordering-Clean-Architecture]] |

---

## 🕒 Cập nhật gần đây

```dataview
TABLE WITHOUT ID file.link AS "Note", type AS "Loại", status AS "Trạng thái", updated AS "Cập nhật"
FROM "01-Languages-Runtimes" OR "02-Architecture-Design" OR "03-Data-Storage" OR "04-DevOps-Infra" OR "05-Projects-POCs"
SORT file.mtime DESC
LIMIT 10
```

## ✍️ Đang viết dở (draft / review)

```dataview
TABLE WITHOUT ID file.link AS "Note", status AS "Trạng thái", topics AS "Chủ đề"
FROM "01-Languages-Runtimes" OR "02-Architecture-Design" OR "03-Data-Storage" OR "04-DevOps-Infra" OR "05-Projects-POCs"
WHERE status = "draft" OR status = "review"
SORT updated ASC
```

## 🌱 Stub notes — viết khi cần (Just-In-Time)

```dataview
LIST
FROM "01-Languages-Runtimes" OR "02-Architecture-Design" OR "03-Data-Storage" OR "04-DevOps-Infra" OR "05-Projects-POCs"
WHERE status = "stub"
SORT file.name ASC
```

## ☕→🟣 Bảng ánh xạ Java ↔ .NET (tự sinh từ frontmatter `java_equivalent`)

```dataview
TABLE WITHOUT ID file.link AS "Khái niệm .NET", java_equivalent AS "Tương đương Java / Spring"
FROM "01-Languages-Runtimes" OR "02-Architecture-Design" OR "03-Data-Storage"
WHERE java_equivalent AND java_equivalent != ""
SORT file.name ASC
```

## 📊 Thống kê theo loại

```dataview
TABLE WITHOUT ID type AS "Loại", length(rows) AS "Số note"
FROM "01-Languages-Runtimes" OR "02-Architecture-Design" OR "03-Data-Storage" OR "04-DevOps-Infra" OR "05-Projects-POCs"
GROUP BY type
SORT length(rows) DESC
```

## 🕳️ Note được nhắc tới nhưng chưa tạo

> [!tip] Wikilink tới note chưa tồn tại = "danh sách việc cần viết" tự nhiên.
> Xem trong **Graph View** (bật *Existing files only* = off) hoặc dùng plugin core **Outgoing links → Unlinked**.
> Chỉ tạo note khi thật sự cần — dùng template [[stub-template]] hoặc [[note-template]].
