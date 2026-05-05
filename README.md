# Keygen-010-Editor

test license:

<img width="1221" height="753" alt="image" src="https://github.com/user-attachments/assets/3bc4b710-b0c6-439a-89c0-43c977d026c0" />

Ta đi vào phân tích logic license:

<img width="1051" height="599" alt="image" src="https://github.com/user-attachments/assets/62d32ee2-0237-4446-822e-f12723011067" />

Nhận thấy chương trình yêu cầu điều kiện `n524 == 0xDB` mới xác nhận là license chính xác

`n524` lại là kết quả của hàm `License_Verify`:

<img width="646" height="201" alt="image" src="https://github.com/user-attachments/assets/3974a27d-50ad-4b91-b9ce-8773cc60e53c" />


Để vào case 45 thì `License_ValidateKey` phải trả về kết quả 45

<img width="836" height="513" alt="image" src="https://github.com/user-attachments/assets/0f35039f-1dc9-43f8-b244-08814fff8b57" />


Ta sẽ phải viết keygen sao cho thỏa mãn các điều kiện trên.

Đi vào chi tiết:

hàm `License_SetUserName`:

<img width="870" height="592" alt="image" src="https://github.com/user-attachments/assets/2e59e7c2-234f-47ed-9678-6d92204a3a99" />

<img width="1053" height="567" alt="image" src="https://github.com/user-attachments/assets/c1795e5b-c445-43fe-8d58-56eec279806f" />

<img width="846" height="218" alt="image" src="https://github.com/user-attachments/assets/f03f1676-04d7-49c6-9ae2-26024e973ebc" />


ta thấy chương trình dùng `QString::operator=` để gán username vào struct biến global

tiếp tục `License_SetKey`:

<img width="1060" height="595" alt="image" src="https://github.com/user-attachments/assets/a30c9e26-207b-49b1-9443-e415d0e392fe" />

<img width="1001" height="155" alt="image" src="https://github.com/user-attachments/assets/7ac477f4-ed2f-42dc-8671-6153533bd434" />

hàm `License_SetKey` cũng tương tự.

Tiếp đến hàm `License_ValidateKey`:

<img width="984" height="544" alt="image" src="https://github.com/user-attachments/assets/c765bda2-37df-49e8-acff-2cebafc60e4c" />

<img width="1007" height="536" alt="image" src="https://github.com/user-attachments/assets/facfd9c6-5dde-4565-9268-8f656142ef55" />

ở đây chương trình dùng `QString::operator==` để check username và vòng lặp để check licensekey có nằm trong blacklist hay không

tiếp đến là logic chính của check license:

```c
    v14 = v46;
    v15 = v44;
    v16 = v41;
    v17 = v40;
    v18 = v39;
    v19 = v42;
    switch ( v42 )
    {
      case 0x9C:
        v20 = (unsigned __int8)(v44 ^ v41) + ((unsigned __int8)(v46 ^ v40) << 8);
        *(_DWORD *)(a1 + 108) = (unsigned __int8)License_DecodeByteTransform((unsigned __int8)(v45 ^ v39));
        v21 = License_DecodeChecksumValue(v20);
        *(_DWORD *)(a1 + 112) = v21;
        n2 = *(_DWORD *)(a1 + 108);
        if ( n2 && (unsigned int)v21 - 1 <= 0x3E7 )
        {
          n255_2 = 0;
          if ( n2 < 2 )
            n255_2 = *(_DWORD *)(a1 + 108);
LABEL_26:
          QString::toUtf8(a1 + 24, v38);
          v25 = *(_DWORD *)(a1 + 112);
          LOBYTE(v4) = v19 != -4;
          v26 = QByteArray::data((QByteArray *)v38);
          n5999655 = License_ComputeHash((__int64)v26, v4, n255_2, v25);
          if ( n5999655_1 == (_BYTE)n5999655
            && (_BYTE)v15 == BYTE1(n5999655)
            && v45 == BYTE2(n5999655)
            && v46 == HIBYTE(n5999655) )
          {
            if ( v19 == -100 )
            {
              if ( n17_1 > *(_DWORD *)(a1 + 108) )
              {
                n78 = 78;
LABEL_41:
                QByteArray::~QByteArray((QByteArray *)v38);
                return n78;
              }
LABEL_33:
              n78 = 45;
              goto LABEL_41;
            }
            if ( v19 == -4 )
            {
              v29 = License_VerifyDateLogic(v39 + (v40 << 8) + (v41 << 16), n5999655);
              if ( v29 )
              {
                *(_DWORD *)(a1 + 104) = v29;
                n78 = 147;
                goto LABEL_41;
              }
            }
            else if ( n255_3 )
            {
              if ( n255_1 > n255_3 )
              {
                n78 = 78;
                goto LABEL_41;
              }
              goto LABEL_33;
            }
          }
          n78 = 231;
          goto LABEL_41;
        }
        break;
      case 0xFC:
        n255_2 = 255;
        *(_DWORD *)(a1 + 108) = 255;
        *(_DWORD *)(a1 + 112) = 1;
        *(_DWORD *)(a1 + 128) = 1;
        goto LABEL_26;
      case 0xAC:
        *(_DWORD *)(a1 + 108) = 2;
        v24 = License_DecodeChecksumValue((unsigned __int16)((unsigned __int8)(v15 ^ v16)
                                                           + ((unsigned __int8)(v14 ^ v17) << 8)));
        *(_DWORD *)(a1 + 112) = v24;
        if ( (unsigned int)v24 - 1 <= 0x1387 )
        {
          n255_3 = License_VerifyDateLogic(
                     (v45 ^ v18) + ((v15 ^ v48) << 16) + ((v47 ^ (unsigned int)n5999655_1) << 8),
                     5999655LL);
          *(_DWORD *)(a1 + 132) = n255_3;
          n255_2 = n255_3;
          goto LABEL_26;
        }
        break;
    }
    return 231LL;
  }
```

Ta thấy chương trình switch case `v42` để kiểm tra các loại license

trỏ vào `v42` ta thấy:

<img width="873" height="212" alt="image" src="https://github.com/user-attachments/assets/f8f7de78-979b-4f66-af8f-7700b6e029bc" />

thấy rằng `v42` là byte hex thứ 4 trong license, vậy chương trình chọn kiểu license thông qua đây

để luồng vào case `0x9C` thì license phải có dạng `AAAA-AA9C-AAAA-AAAA`.

<img width="1214" height="745" alt="image" src="https://github.com/user-attachments/assets/11e3f521-e206-4802-ac01-4b3f4158b0fd" />

Trỏ vào các biến được sử dụng để biết bị trí của nó trong key:

v46:

<img width="885" height="160" alt="image" src="https://github.com/user-attachments/assets/904ca264-b321-48b6-bdec-ee05e221f7c4" />

v44:

<img width="832" height="188" alt="image" src="https://github.com/user-attachments/assets/6503f377-1248-4c57-b3f6-ac32e51a37cb" />

tiếp đến là đầy đủ các phần tử trong license key ứng với các biến:

<img width="899" height="171" alt="image" src="https://github.com/user-attachments/assets/05b7d3b0-1b1a-4ace-89a7-c5a79f92fa8a" />

trước tiên là ta phải thỏa mãn điều kiện thứ nhất để có thể tiếp tục debug:

<img width="1025" height="520" alt="image" src="https://github.com/user-attachments/assets/d9a5dc21-b40a-431e-a30f-8869da94be25" />

viết script nhỏ bruteforce bằng python để "tạm thời" pass qua bước này:

```python
key="AAAA-AA9C-AAAA-AAAA"
kay="3940-4142-4344-4546"

"""
(v44,v41,v46,v40) -> v20
(v45,v39) -> a1[108] -> n2
(v20) -> v21 -> a1[112]
cmp n2, 0x3E7
"""

n2=0xf2
for v40 in range(256):
    for v41 in range(256):
        for v44 in range(256):
            for v46 in range(256):
                v20=((v44^v41)+(v46^v40)<<8)%0xffff
                v1 = (((v20 ^ 0x7892) + 0x4D30)&0xffff ^ 0x3421)%0xffff
                if v1%11:
                    v21=0
                else:
                    v21=v1//11
                if n2 and (v21-1)&0xffff <= 0x3e7:
                    print(v21)
                    print(f"found: {v40}, {v41}, {v44}, {v46}")                
```

<img width="680" height="466" alt="image" src="https://github.com/user-attachments/assets/fc94a809-186b-442c-aa54-7bf252c8ef2f" />

vậy ta chọn tạm `AA00-009C-AA00-AA9A`

<img width="908" height="394" alt="image" src="https://github.com/user-attachments/assets/5afabc2e-5b0b-469b-afc2-8a3e5d2ea152" />










