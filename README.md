<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ที่เก็บฉลากเอราวัณเคมีเกษตร</title>
    <style>
        /* ตกแต่งพื้นฐาน */
        body {
            font-family: 'Arial', sans-serif;
            background-image: url(ss1.jpg);
            margin: 0;
            padding: 0;
            color: #0f0f0f;
        }

        .container {
            max-width: 800px;
            margin: 50px auto;
            padding: 20px;
            
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            border-radius: 8px;
        }

        h1 {
            text-align: center;
            color: #089c35;
        }

        /* ตกแต่งช่องค้นหา */
        .search-bar {
            display: flex;
            justify-content: center;
            margin: 20px 0;
        }

        input[type="text"] {
            width: 70%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px 0 0 4px;
            font-size: 16px;
        }

        button {
            padding: 10px 20px;
            border: none;
            background-color: #06793f;
            color: #fff;
            font-size: 16px;
            cursor: pointer;
            border-radius: 0 4px 4px 0;
            transition: background-color 0.3s;
        }

        button:hover {
            background-color: #07f707;
        }

        /* ตกแต่งตาราง */
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }

        table, th, td {
            border: 1px solid #ddd;
        }

        th {
            background-color: #04581d;
            color: white;
            padding: 10px;
        }

        td {
            padding: 10px;
            text-align: center;
        }

        tr {
            background-color: #f2f2f2;
        }

      

        /* สไตล์สำหรับกรณีไม่มีสินค้า */
        .no-results {
            text-align: center;
            color: #f80707;
            
            font-size: 30px;
        }
    </style>
</head>
<body>
    
    <div class="container">
        <h1>ค้นหาฉลากสินค้าในคลัง</h1>
        <div class="search-bar">
            <input type="text" id="searchInput" placeholder="ค้นหาสินค้า...">
            <button onclick="searchProduct()">ค้นหา</button>
        </div>
        <table>
            <thead>
                <tr>
                    <th>ชื่อฉลากสินค้า</th>
                    <th>บริษัท</th>
                    <th>ประเภทยา</th>
                    <th>ชั้นวาง</th>
                    <th>ตำแหน่ง</th>
                </tr>
            </thead>
            <tbody id="productTable">
                <!-- สินค้าจะถูกแสดงที่นี่ -->
            </tbody>
        </table>
        <div id="noResults" class="no-results" style="display: none;">ไม่พบข้อมูล</div>
    </div>

    <script>
        // ข้อมูลสินค้า (ใช้แทนฐานข้อมูล)
        const products = [
        { name: "อะซีโทคลอร์ ขนาด 500 ml.", category: "เอราวัณ", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C1" },
            { name: "ไซแอมเพนดิ ขนาด 1000 ml.", category: "สยามแอ็ก", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C2" },
            { name: "ไซแอมนิล 36 อีซี ขนาด 1000 ml.", category: "สยามแอ็ก", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C2" },
            { name: "ไซแอมเซทโท ", category: "สยามแอ็ก", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C2" },
            { name: "เทโทซิน 72 ขนาด 1000 ml.", category: "สยามแอ็ก", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C2" },
            { name: "แพ็คเก็จธาลิน", category: "APC", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C3" },
            { name: "แพ็คเก็จบิวทาคลอร์ ขนาด 1000 ml.", category: "APC", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C3" },
            { name: "แม็คราโด้ ขนาด 500 ml.", category: "APC", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C3" },
            { name: "โซแพ็ค ขนาด 1000 ml.", category: "APC", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C3" },
            { name: "ไอยราฟอพ 15 อีซี ขนาด 1000 ml.", category: "Aiyara", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C4" },
            { name: "ไอยราฟีน็อก ขนาด 500 ml.", category: "Aiyara", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C4" },
            { name: "ไอยราล็อคซี ขนาด 500 ml.", category: "Aiyara", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C4" },
            { name: "ไอยราซาโล ขนาด 500 ml.", category: "Aiyara", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C4" },
            { name: "ไอยราเอ็นเธอร์ ขนาด 500 ml.", category: "Aiyara", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C4" },
            { name: "ไอยราซาเฟน ขนาด 500 ml.", category: "Aiyara", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C4" },
            { name: "เอราธาลีน ขนาด 1000 ml.", category: "เอราวัณ", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C5" },
            { name: "เอราโซนิล ขนาด 1000 ml.", category: "เอราวัณ", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C5" },
            { name: "ไซแอมโพรพาคลอร์ 70 ขนาด 1000 ml.", category: "จ็สยามแอ็ก", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C6" },
            { name: "ไซแอมพรี ขนาด 1000 ml.", category: "สยามแอ็ก", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C6" },
            { name: "เฮบล็ออคนอร์ท ขนาด 500 ml.", category: "APC", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C7" },
            { name: "แพ็คเก็จซาเฟน ขนาด 500 ml.", category: "APC", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C7" },
            { name: "แพ็คเก็จซาเฟน ขนาด 1000 ml.", category: "APC", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C7" },
            { name: "โพรเฟี๊ยต ขนาด 1000 ml.", category: "APC", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C7" },
            { name: "แพ็คโซนิล ขนาด 1000 ml.", category: "APC", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C7" },
            { name: "ไอยราพรี ขนาด 1000 ml.", category: "Aiyara", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C8" },
            { name: "ไอยรามาโซน ขนาด 500 ml.", category: "Aiyara", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C8" },
            { name: "ไอยรามาโซน ขนาด 500 ml.", category: "Aiyara", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C8" },
            { name: "ไอยราซีโนน ขนาด 500 ml.", category: "Aiyara", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C8" },
            { name: "เอราซีน 50 เอสซี ขนาด 1000 ml.", category: "เอราวัณ", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C9" },
            { name: "เอราการ์ด ขนาด 1000 ml.", category: "เอราวัณ", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C9" },
            { name: "ไซแอมเซทโท ขนาด 1000 ml.", category: "สยามแอ็ก", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C10" },
            { name: "ไซแอมสตาร์ ขนาด 1000 ml.", category: "สยามแอ็ก", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C10" },
            { name: "ไซแอมสตาร์ ขนาด 500 ml.", category: "สยามแอ็ก", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C10" },
            { name: "แพ็คเก็จสตาร์ ขนาด 500 ml.", category: "APC", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C11" },
            { name: "เอเลฟอบ 15 อีซี ขนาด 500 ml.", category: "APC", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C11" },
            { name: "มัปเป็ดเทียร์ ขนาด 500 ml.", category: "APC", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C11" },
            { name: "มัปเป็ดเทียร์ ขนาด 1000 ml.", category: "APC", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C11" },
            { name: "ไอยราฟอพ 15 อีซี ขนาด 500 ml.", category: "Aiyara", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C12" },
            { name: "บิสโซ่ ขนาด 500 ml.", category: "Aiyara", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C12" },
            { name: "ไอยราฟิกซ์ ขนาด 500 ml.", category: "Aiyara", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C12" },
            { name: "เอรากราสวีด ขนาด 500 ml.", category: "เอราวัณ", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C13" },
            { name: "เอราฟอพโกลด์ ขนาด 500 ml.", category: "เอราวัณ", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C13" },
            { name: "เอราฟอพโกลด์ ขนาด 1000 ml.", category: "เอราวัณ", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C13" },
            { name: "เอราโพรพาคลอร์ 70 ขนาด 1000 ml.", category: "เอราวัณ", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C13" },
            { name: "ไซแอมซีน 50 เอสซี ขนาด 1000 ml.", category: "สยามแอ็ก", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C14" },
            { name: "ไซแอมทรีน 50 เอสซี ขนาด 1000 ml.", category: "สยามแอ็ก", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C14" },
            { name: "แพ็คสตาร์ ขนาด 1000 ml.", category: "APC", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C15" },
            { name: "แพ็คเก็จสตาร์ อีซี ขนาด 500 ml.", category: "APC", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C15" },
            { name: "แพ็คเก็จสตาร์ อีซี ขนาด 1000 ml.", category: "APC", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C15" },
            { name: "ไอยราฟิกซ์ ขนาด 250 ml.", category: "Aiyara", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C16" },
            { name: "ฟาสท์ ขนาด 100 ml.", category: "Aiyara", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C16" },
            { name: "ไอบล็อคซี ขนาด 100 ml.", category: "Aiyara", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C16" },
            { name: "เอราเอมีน 84 ขนาด 4000 ml.", category: "เอราวัณ", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C17" },
            { name: "เอราทรีน 50 เอสซี ขนาด 1000 ml.", category: "เอราวัณ", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C17" },
            { name: "ไซแอมการ์ด โกลด์ ขนาด 1000 ml.", category: "สยามแอ็ก", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C18" },
            { name: "ไซแอมการ์ด โกลด์ ขนาด 500 ml.", category: "สยามแอ็ก", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C18" },
            { name: "ไซแอมการ์ด โกลด์ ขนาด 100 ml.", category: "สยามแอ็ก", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C18" },
            { name: "แพ็คเก็จพลัส 70 ขนาด 1000 ml.", category: "APC", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C19" },
            { name: "แพ็คเก็จพรี ขนาด 1000 ml.", category: "APC", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C19" },
            { name: "ลูเปอร์ ขนาด 1000 ml.", category: "APC", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C19" },
            { name: "ลูเปอร์ ขนาด 500 ml.", category: "APC", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C19" },
            { name: "ไอยราคอมบี 50 เอสซี ขนาด 1000 ml.", category: "Aiyara", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C20" },
            { name: "ไอยราโซนิล ขนาด 1000 ml.", category: "Aiyara", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C20" },
            { name: "เอราเอมีน 84 ขนาด 500 ml.", category: "เอราวัณ", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C21" },
            { name: "ฮุชเช่อร์ ขนาด 1000 ml.", category: "สยามแอ็ก", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C22" },
            { name: "แพ็คเก็จพลัส 70 ขนาด 1000 ml.", category: "APC", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C23" },
            { name: "แพ็คเก็จสตาร์ ขนาด 250 ml.", category: "APC", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C23" },
            { name: "ไอยราเอมีน 84 ขนาด 1000 ml.", category: "Aiyara", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C24" },
            { name: "ไอยราเพนดิ ขนาด 1000 ml.", category: "Aiyara", quantity: "กำจัดวัชพืช", location: "C", location2: "ช่อง C24" },
        { name: "เอรามิน ขนาด 100 ml.", category: "เอราวัณ", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F1" },
        { name: "เอรามิน ขนาด 500 ml.", category: "เอราวัณ", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F1" },
        { name: "เอราโพรฟอส ขนาด 1000 ml.", category: "เอราวัณ", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F1" },
        { name: "ไซแอมเม็คติน ขนาด 500 ml.ขวด PET", category: "สยามแอ็ก", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F2" },
        { name: "ไซแอมเม็คติน ขนาด 1000 ml.ขวด PET", category: "สยามแอ็ก", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F2" },
        { name: "ไซแอมเม็คติน ขนาด 500 ml. ขวดพลาสติก", category: "สยามแอ็ก", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F2" },
        { name: "ไซแอมเม็คติน ขนาด 1000 ml.ขวดพลาสติก", category: "สยามแอ็ก", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F2" },
        { name: "บีลินเมเจอร์พลัส ขนาด 250 ml.", category: "APC", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F3" },
        { name: "บีลินเมเจอร์พลัส ขนาด 500 ml.", category: "APC", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F3" },
        { name: "แพ็จเก็จโทเอต เอสแอล ขนาด 1000 ml.", category: "APC", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F3" },
        { name: "ไอยราบูต้า 40 เอสซี ขนาด 1000 ml.", category: "Aiyara", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F4" },
        { name: "ไอลิค ขนาด 1000 ml.ขวด PET", category: "Aiyara", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F4" },
        { name: "ไอลิค ขนาด 1000 ml.ขวดพลาสติก", category: "Aiyara", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F4" },
        { name: "เอรามิลิน ขนาด 500 ml.", category: "เอราวัณ", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F5" },
        { name: "เอราทริน 2.5 ขนาด 500 ml.", category: "เอราวัณ", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F5" },
        { name: "เอราทริน 2.5 ขนาด 1000 ml.", category: "เอราวัณ", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F5" },
        { name: "ไซแอมเม็คติน ใบแทรก", category: "สยามแอ็ก", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F6" },
        { name: "ไซแอมเม็คติน ขนาด 100 ml.", category: "สยามแอ็ก", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F6" },
        { name: "แพ็จเก็จมิฟอส เอสแอล ขนาด 1000 ml.", category: "APC", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F7" },
        { name: "แพ็จเก็จมิฟอส เอสแอล ขนาด 500 ml.", category: "APC", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F7" },
        { name: "มิลินเจอร์พลัส เอสแอล ขนาด 500 ml.", category: "APC", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F7" },
        { name: "แพ็คเก็จโมเมท เอสแอล ขนาด 1000 ml.", category: "APC", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F7" },
        { name: "ไอยรามอล 83 ขนาด 1000 ml.", category: "Aiyara", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F8" },
        { name: "เอเต้ ขนาด 1000 ml.", category: "Aiyara", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F8" },
        { name: "ไอยราซัลแฟน ขนาด 1000 ml.", category: "Aiyara", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F8" },
        { name: "เอราเช็ค 35 อีซี ขนาด 1000 ml.", category: "เอราวัณ", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F9" },
        { name: "เอราเช็ค 35 อีซี ขนาด 500 ml.", category: "เอราวัณ", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F9" },
        { name: "เอราเช็ค 35 อีซี ขนาด 100 ml.", category: "เอราวัณ", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F9" },
        { name: "เอรานูฟอส ขนาด 500 ml.", category: "เอราวัณ", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F9" },
        { name: "เอรานูฟอส ขนาด 100 ml.", category: "เอราวัณ", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F9" },
        { name: "ไซแอมเมทบลู ขนาด 200 ml.ขวด PET", category: "สยามแอ็ก", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F10" },
        { name: "ไซแอมเมทบลู ขนาด 1000 ml.ขวด PET", category: "สยามแอ็ก", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F10" },
        { name: "ไซแอมเมทบลู ขนาด 100 ml.ขวด พลาสติก", category: "สยามแอ็ก", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F10" },
        { name: "ไซแอมเมทบลู ขนาด 250 ml.ขวด พลาสติก", category: "สยามแอ็ก", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F10" },
        { name: "แพ็คเก็จแลมด้า ขนาด 1000 ml.", category: "APC", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F11" },
        { name: "แพ็คเก็จมอล 83 ขนาด 1000 ml.", category: "APC", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F11" },
        { name: "แพ็คเก็จมอล 83 ขนาด 500 ml.", category: "APC", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F11" },
        { name: "แม็คนั่ม ขนาด 1000 ml.", category: "Aiyara", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F12" },
        { name: "แม็คนั่ม ขนาด 500 ml.", category: "Aiyara", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F12" },
        { name: "ไอยรามีทราส ขนาด 1000 ml.", category: "Aiyara", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F12" },
        { name: "สเปซ ขนาด 1000 ml.", category: "Aiyara", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F12" },
        { name: "บลูด็อกซ์ ขนาด 100 ml.", category: "เอราวัณ", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F13" },
        { name: "บลูด็อกซ์ ขนาด 5 กรัม x 40 ซอง", category: "เอราวัณ", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F13" },
        { name: "เอราเม็คติน ขนาด 1000 ml.", category: "เอราวัณ", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F13" },
        { name: "เอราเม็คติน ขนาด 100 ml.", category: "เอราวัณ", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F13" },
        { name: "ไซแอมโพรทริน ขนาด 1000 ml.ขวดพลาสติก", category: "สยามแอ็ก", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F14" },
        { name: "ไซแอมโพรทริน ใบแทรก ", category: "เอราวัณ", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F14" },
        { name: "ไซแอมมาลา ขนาด 1000 ml.ขวด PET", category: "สยามแอ็ก", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F14" },
        { name: "ไซแอมมาลา ขนาด 1000 ml.ขวดพลาสติก", category: "สยามแอ็ก", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F14" },
        { name: "เอเลเช็ค ขนาด 100 ml.", category: "APC", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F15" },
        { name: "เอเลเช็ค ขนาด 500 ml.", category: "APC", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F15" },
        { name: "เอเลเช็ค ขนาด 1000 ml.", category: "APC", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F15" },
        { name: "แพ็คเก็จ เมทโกลด์ ขนาด 1000 ml. ขวดแก้ว", category: "APC", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F15" },
        { name: "แพ็คเก็จ เมทโกลด์ ขนาด 1000 ml. ขวดพลาสติก", category: "APC", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F15" },
        { name: "ไอยราไซฟอส ขนาด 1000 ml.", category: "Aiyara", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F16" },
        { name: "สไตไซฟอส ขนาด 1000 ml.", category: "Aiyara", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F16" },
        { name: "ไอยราไซฟอส ขนาด 500 ml.", category: "Aiyara", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F16" },
        { name: "เอราเมท Gold ขนาด 1000 ml.", category: "เอราวัณ", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F17" },
        { name: "เซเรน ขนาด 1000 ml.", category: "เอราวัณ", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F17" },
        { name: "เอราเมท Gold ขนาด 100 ml.", category: "เอราวัณ", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F17" },
        { name: "เอราโพรริน Gold ขนาด 100 ml.", category: "เอราวัณ", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F17" },
        { name: "ไซแอมนูรอน ขนาด 500 ml.ขวด PET", category: "สยามแอ็ก", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F18" },
        { name: "ไซแอมนูรอน ขนาด 100 ml.ขวด PET", category: "สยามแอ็ก", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F18" },
        { name: "ไซแอมนูฟอส ขนาด 100 ml.ขวด PET", category: "สยามแอ็ก", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F18" },
        { name: "ไซแอมนูฟอส ขนาด 500 ml.ขวด PET", category: "สยามแอ็ก", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F18" },
        { name: "แพ็คเก็จติน ขนาด 1000 ml. ขวดแก้ว", category: "APC", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F19" },
        { name: "แพ็คเก็จติน ขนาด 1000 ml. ขวดพลาส", category: "APC", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F19" },
        { name: "แพ็คเก็จติน ขนาด 500 ml.", category: "APC", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F19" },
        { name: "แพ็คเก็จติน ขนาด 100 ml.", category: "APC", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F19" },
        { name: "ไอยราเช็ค 35 อีซี ขนาด 1000 ml.ฉลากสีฟ้า", category: "Aiyara", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F20" },
        { name: "ไอยราเช็ค 35 อีซี่ ขนาด 1000 ml.ฉลากสีม่วง", category: "Aiyara", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F20" },
        { name: "ไอยราเช็ค 35 อีซี ขนาด 500 ml.", category: "Aiyara", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F20" },
        { name: "เอราโพรทริน ขนาด 1000 ml.", category: "เอราวัณ", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F21" },
        { name: "เอราโพรทริน ขนาด 500 ml.", category: "เอราวัณ", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F21" },
        { name: "เอราโพรทริน ขนาด 500 ml.", category: "สยามแอ็ก", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F22" },
        { name: "แพ็คเก็จนูรอน ขนาด 100 ml.", category: "APC", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F23" },
        { name: "แพ็คเก็จนูรอน ขนาด 500 ml.", category: "APC", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F23" },
        { name: "แพ็คเก็จซัลแฟน ขนาด 1000 ml.", category: "APC", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F23" },
        { name: "แพ็คเก็จซัลแฟน ขนาด 500 ml.", category: "APC", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F23" },
        { name: "แพ็คเก็จซัลแฟน ขนาด 100 ml.", category: "APC", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F23" },
        { name: "ไอยราเม็คติน ขนาด 1000 ml.", category: "Aiyara", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F24" },
        { name: "โพรฟีรอน ขนาด 500 ml.", category: "Aiyara", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F24" },
        { name: "ไอยรามิลิน ขนาด 500 ml.", category: "Aiyara", quantity: "กำจัดศัตรูพืช", location: "F", location2: "ช่อง F24" }
            
        ];

        // ฟังก์ชันค้นหา
        function searchProduct() {
            const input = document.getElementById("searchInput").value.toLowerCase();
            const tableBody = document.getElementById("productTable");
            const noResults = document.getElementById("noResults");
            tableBody.innerHTML = "";
            noResults.style.display = "none";

            const filteredProducts = products.filter(product =>
                product.name.toLowerCase().includes(input) ||
                product.category.toLowerCase().includes(input)  
            );

            if (filteredProducts.length === 0) {
                noResults.style.display = "block";
                return;
            }

            filteredProducts.forEach(product => {
                const row = `
                    <tr>
                        <td>${product.name}</td>
                        <td>${product.category}</td>
                        <td>${product.quantity}</td>
                        <td>${product.location}</td>
                        <td>${product.location2}</td>
                       
                        
                    </tr>
                `;
                tableBody.innerHTML += row;
            });
        }
    </script>
</body>
</html>
