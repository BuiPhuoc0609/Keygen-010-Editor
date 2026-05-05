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
                v20=((v44^v41)+((v46^v40)<<8))%0xffff
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

<img width="885" height="526" alt="image" src="https://github.com/user-attachments/assets/f9893946-a412-4fe7-a1e6-657ba838b013" />

Xét các tham só truyền vào `License_ComputeHash`:

`username` chính là name ta nhập vào ô license

ta biết rằng `v42==0x9C` nên mặc định `v4==1`

<img width="626" height="48" alt="image" src="https://github.com/user-attachments/assets/52ae5f69-6318-4a2e-99c0-d6ae1924bfba" />

ở đây ta thấy n255_2 chỉ có thể là 0 hoặc 1

mà:

<img width="613" height="170" alt="image" src="https://github.com/user-attachments/assets/fb754516-7182-4e13-a346-b4e413376d32" />

vậy để vào luồng trả về 45 thì ta phải để `a1+108` càng lớn càng tốt, như vậy `v255_2` mặc định luôn bằng 0 và `n2` phải luôn lớn hơn hoặc bằng `0x11`.

Còn `v25` bị ràng buộc ở khá nhiều chỗ:

<img width="802" height="483" alt="image" src="https://github.com/user-attachments/assets/464d74c2-3c74-4fe7-a6d7-f191e3e5b4f3" />

như vậy ta sẽ thử bruteforce để hoàn thành key chính xác.

```python
key="AAAA-AA9C-AAAA-AAAA"
kay="3940-4142-4344-4546"

"""
(v44,v41,v46,v40) -> v20
(v45,v39) -> a1[108] -> n2
(v20) -> v21 -> a1[112]
cmp n2, 0x3E7
"""

dword_C0F0=[0x39CB44B8, 0x23754F67, 0x5F017211, 0x3EBB24DA, 0x351707C6, 0x63F9774B, 0x17827288, 0x0FE74821, 0x5B5F670F, 0x48315AE8, 0x785B7769, 0x2B7A1547, 0x38D11292, 0x42A11B32, 0x35332244, 0x77437B60, 0x1EAB3B10, 0x53810000, 0x1D0212AE, 0x6F0377A8, 0x43C03092, 0x2D3C0A8E, 0x62950CBF, 0x30F06FFA, 0x34F710E0, 0x28F417FB, 0x350D2F95, 0x5A361D5A, 0x15CC060B, 0x0AFD13CC, 0x28603BCF, 0x3371066B, 0x30CD14E4, 0x175D3A67, 0x6DD66A13, 0x2D3409F9, 0x581E7B82, 0x76526B99, 0x5C8D5188, 0x2C857971, 0x15F51FC0, 0x68CC0D11, 0x49F55E5C, 0x275E4364, 0x2D1E0DBC, 0x4CEE7CE3, 0x32555840, 0x112E2E08, 0x6978065A, 0x72921406, 0x314578E7, 0x175621B7, 0x40771DBF, 0x3FC238D6, 0x4A31128A, 0x2DAD036E, 0x41A069D6, 0x25400192, 0x00DD4667, 0x6AFC1F4F, 0x571040CE, 0x62FE66DF, 0x41DB4B3E, 0x3582231F, 0x55F6079A, 0x1CA70644, 0x1B1643D2, 0x3F7228C9, 0x5F141070, 0x3E1474AB, 0x444B256E, 0x537050D9, 0x0F42094B, 0x2FD820E6, 0x778B2E5E, 0x71176D02, 0x7FEA7A69, 0x5BB54628, 0x19BA6C71, 0x39763A99, 0x178D54CD, 0x01246E88, 0x3313537E, 0x2B8E2D17, 0x2A3D10BE, 0x59D10582, 0x37A163DB, 0x30D6489A, 0x6A215C46, 0x0E1C7A76, 0x1FC760E7, 0x79B80C65, 0x27F459B4, 0x799A7326, 0x50BA1782, 0x2A116D5C, 0x63866E1B, 0x3F920E3C, 0x55023490, 0x55B56089, 0x2C391FD1, 0x2F8035C2, 0x64FD2B7A, 0x4CE8759A, 0x518504F0, 0x799501A8, 0x3F5B2CAD, 0x38E60160, 0x637641D8, 0x33352A42, 0x51A22C19, 0x085C5851, 0x032917AB, 0x2B770AC7, 0x30AC77B3, 0x2BEC1907, 0x035202D0, 0x0FA933D3, 0x61255DF3, 0x22AD06BF, 0x58B86971, 0x5FCA0DE5, 0x700D6456, 0x56A973DB, 0x5AB759FD, 0x330E0BE2, 0x5B3C0DDD, 0x495D3C60, 0x53BD59A6, 0x4C5E6D91, 0x49D9318D, 0x103D5079, 0x61CE42E3, 0x7ED5121D, 0x14E160ED, 0x212D4EF2, 0x270133F0, 0x62435A96, 0x1FA75E8B, 0x6F092FBE, 0x4A000D49, 0x57AE1C70, 0x004E2477, 0x561E7E72, 0x468C0033, 0x5DCC2402, 0x78507AC6, 0x58AF24C7, 0x0DF62D34, 0x358A4708, 0x3CFB1E11, 0x2B71451C, 0x77A75295, 0x56890721, 0x0FEF75F3, 0x120F24F1, 0x01990AE7, 0x339C4452, 0x27A15B8E, 0x0BA7276D, 0x60DC1B7B, 0x4F4B7F82, 0x67DB7007, 0x4F4A57D9, 0x621252E8, 0x20532CFC, 0x6A390306, 0x18800423, 0x19F3778A, 0x462316F0, 0x56AE0937, 0x43C2675C, 0x65CA45FD, 0x0D604FF2, 0x0BFD22CB, 0x3AFE643B, 0x3BF67FA6, 0x44623579, 0x184031F8, 0x32174F97, 0x4C6A092A, 0x5FB50261, 0x01650174, 0x33634AF1, 0x712D18F4, 0x6E997169, 0x5DAB7AFE, 0x7C2B2EE8, 0x6EDB75B4, 0x5F836FB6, 0x3C2A6DD6, 0x292D05C2, 0x052244DB, 0x149A5F4F, 0x5D486540, 0x331D15EA, 0x4F456920, 0x483A699F, 0x3B450F05, 0x3B207C6C, 0x749D70FE, 0x417461F6, 0x62B031F1, 0x2750577B, 0x29131533, 0x588C3808, 0x1AEF3456, 0x0F3C00EC, 0x7DA74742, 0x4B797A6C, 0x5EBB3287, 0x786558B8, 0x00ED4FF2, 0x6269691E, 0x24A2255F, 0x62C11F7E, 0x2F8A7DCD, 0x643B17FE, 0x778318B8, 0x253B60FE, 0x34BB63A3, 0x5B03214F, 0x5F1571F4, 0x1A316E9F, 0x7ACF2704, 0x28896838, 0x18614677, 0x1BF569EB, 0x0BA85EC9, 0x6ACA6B46, 0x1E43422A, 0x514D5F0E, 0x413E018C, 0x307626E9, 0x01ED1DFA, 0x49F46F5A, 0x461B642B, 0x7D7007F2, 0x13652657, 0x6B160BC5, 0x65E04849, 0x1F526E1C, 0x5A0251B6, 0x2BD73F69, 0x2DBF7ACD, 0x51E63E80, 0x5CF2670F, 0x21CD0A03, 0x5CFF0261, 0x33AE061E, 0x3BB6345F, 0x5D814A75, 0x257B5DF4, 0x0A5C2C5B, 0x16A45527, 0x16F23945, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000, 0x00000000]

def computehash(username, a2, n255, a4):
    v5 = 0
    v7 = len(username)

    v8 = 0
    v9 = 0
    v10 = 15 * a4
    v11 = 0
    v12 = 17 * n255

    while v8 < v7:
        v13 = ord(username[v8].upper())
        v14 = v13

        v15 = dword_C0F0[v12 & 0xff]
        v16 = dword_C0F0[v10 & 0xff]

        v17 = (v5 + dword_C0F0[v13 & 0xff]) & 0xffffffff

        if a2:
            v18 = dword_C0F0[(v14 + 13) & 0xff] ^ v17
            v19 = (v14 + 47) & 0xff
            v20 = v9 & 0xff
        else:
            v18 = dword_C0F0[(v14 + 63) & 0xff] ^ v17
            v19 = (v14 + 23) & 0xff
            v20 = v11 & 0xff

        v12 += 9
        v10 += 13
        v9 += 19
        v11 += 7
        v8 += 1

        v5 = (v16 + v15 + dword_C0F0[v20] + dword_C0F0[v19] * v18) & 0xffffffff

    return v5
    
"""
username=b"test"
v4=1
n255_2=0
v25=v21
n5999655=computehash(username,v4,n255_2,v25)
print(hex(n5999655))
v43=n5999655&0xff
v44=n5999655>>8&0xff
v45=n5999655>>16&0xff
v46=n5999655>>24&0xff

"""
v42=0x9C
v39=0
for v40 in range(256):
    for v41 in range(256):
        for v44 in range(256):
            for v46 in range(256):
                v20=((v44^v41)+((v46^v40)<<8))%0xffff
                v1 = (((v20 ^ 0x7892) + 0x4D30)&0xffff ^ 0x3421)%0xffff
                if v1%11:
                    v21=0
                else:
                    v21=v1//11
                if (v21-1)&0xffff <= 0x3e7:
                    #print(v21)
                    username="test"
                    v4=1
                    n255_2=0
                    v25=v21
                    n5999655=computehash(username,v4,n255_2,v25)
                    #print(hex(n5999655))
                    v43=n5999655&0xff
                    v44_1=n5999655>>8&0xff
                    v45=n5999655>>16&0xff
                    v46_1=n5999655>>24&0xff

                    a1_108=((((v45^v39)^0x18)+61)^0xa7)&0xff
                    
                    if v44 == v44_1 and v46==v46_1 and a1_108>=0x11:
                        print(v25)
                        print(hex(n5999655))
                        print(f"found: {v39}, {v40}, {v41}, {v42}, {v43}, {v44}, {v45}, {v46}")
                        
```

<img width="653" height="152" alt="image" src="https://github.com/user-attachments/assets/5fdfdb9b-e658-48ba-a3b0-73d8c0fef739" />

<img width="975" height="678" alt="image" src="https://github.com/user-attachments/assets/2ee0aa39-ae51-448e-a2d7-6facd22eab7c" />












