# sheldon1

```console
$ ./sheldon1
Welcome to my fiendish little bomb. You have 6 phases with
which to blow yourself up. Have a nice day!
1

BOOM!!!
The bomb has blown up.
```

Objdump into a file
```console
$ objdump -M intel -d sheldon1 > sheldon1.objdump.txt
```

Analyze the functions using gdb


## Phase 1
`phase_1` function code
```
08048b20 <phase_1>:
 8048b20:	55                   	push   ebp
 8048b21:	89 e5                	mov    ebp,esp
 8048b23:	83 ec 08             	sub    esp,0x8
 8048b26:	8b 45 08             	mov    eax,DWORD PTR [ebp+0x8]
 8048b29:	83 c4 f8             	add    esp,0xfffffff8
 8048b2c:	68 c0 97 04 08       	push   0x80497c0
 8048b31:	50                   	push   eax
 8048b32:	e8 f9 04 00 00       	call   8049030 <strings_not_equal>
 8048b37:	83 c4 10             	add    esp,0x10
 8048b3a:	85 c0                	test   eax,eax
 8048b3c:	74 05                	je     8048b43 <phase_1+0x23>
 8048b3e:	e8 b9 09 00 00       	call   80494fc <explode_bomb>
 8048b43:	89 ec                	mov    esp,ebp
 8048b45:	5d                   	pop    ebp
 8048b46:	c3                   	ret
 8048b47:	90                   	nop
```

Print value of string
```
(gdb) x/s 0x80497c0
0x80497c0: "Public speaking is very easy."
```

```c
phase_1(char *input) {
	if (strings_not_equal(input, "Public speaking is very easy.")) {
		explode_bomb();
	}
}
```

key
```
Public speaking is very easy.
```

## Phase 2
```
08048fd8 <read_six_numbers>:
 8048fd8:	55                   	push   ebp
 8048fd9:	89 e5                	mov    ebp,esp
 8048fdb:	83 ec 08             	sub    esp,0x8
 8048fde:	8b 4d 08             	mov    ecx,DWORD PTR [ebp+0x8]
 8048fe1:	8b 55 0c             	mov    edx,DWORD PTR [ebp+0xc]
 8048fe4:	8d 42 14             	lea    eax,[edx+0x14]
 8048fe7:	50                   	push   eax
 8048fe8:	8d 42 10             	lea    eax,[edx+0x10]
 8048feb:	50                   	push   eax
 8048fec:	8d 42 0c             	lea    eax,[edx+0xc]
 8048fef:	50                   	push   eax
 8048ff0:	8d 42 08             	lea    eax,[edx+0x8]
 8048ff3:	50                   	push   eax
 8048ff4:	8d 42 04             	lea    eax,[edx+0x4]
 8048ff7:	50                   	push   eax
 8048ff8:	52                   	push   edx
 8048ff9:	68 1b 9b 04 08       	push   0x8049b1b
 8048ffe:	51                   	push   ecx
 8048fff:	e8 5c f8 ff ff       	call   8048860 <sscanf@plt>
 8049004:	83 c4 20             	add    esp,0x20
 8049007:	83 f8 05             	cmp    eax,0x5
 804900a:	7f 05                	jg     8049011 <read_six_numbers+0x39>
 804900c:	e8 eb 04 00 00       	call   80494fc <explode_bomb>
 8049011:	89 ec                	mov    esp,ebp
 8049013:	5d                   	pop    ebp
 8049014:	c3                   	ret
 8049015:	8d 76 00             	lea    esi,[esi+0x0]

08048b48 <phase_2>:
 8048b48:	55                   	push   ebp
 8048b49:	89 e5                	mov    ebp,esp
 8048b4b:	83 ec 20             	sub    esp,0x20
 8048b4e:	56                   	push   esi
 8048b4f:	53                   	push   ebx
 8048b50:	8b 55 08             	mov    edx,DWORD PTR [ebp+0x8]
 8048b53:	83 c4 f8             	add    esp,0xfffffff8
 8048b56:	8d 45 e8             	lea    eax,[ebp-0x18]
 8048b59:	50                   	push   eax
 8048b5a:	52                   	push   edx
 8048b5b:	e8 78 04 00 00       	call   8048fd8 <read_six_numbers>
 8048b60:	83 c4 10             	add    esp,0x10
 8048b63:	83 7d e8 01          	cmp    DWORD PTR [ebp-0x18],0x1
 8048b67:	74 05                	je     8048b6e <phase_2+0x26>
 8048b69:	e8 8e 09 00 00       	call   80494fc <explode_bomb>
 8048b6e:	bb 01 00 00 00       	mov    ebx,0x1
 8048b73:	8d 75 e8             	lea    esi,[ebp-0x18]
 8048b76:	8d 43 01             	lea    eax,[ebx+0x1]
 8048b79:	0f af 44 9e fc       	imul   eax,DWORD PTR [esi+ebx*4-0x4]
 8048b7e:	39 04 9e             	cmp    DWORD PTR [esi+ebx*4],eax
 8048b81:	74 05                	je     8048b88 <phase_2+0x40>
 8048b83:	e8 74 09 00 00       	call   80494fc <explode_bomb>
 8048b88:	43                   	inc    ebx
 8048b89:	83 fb 05             	cmp    ebx,0x5
 8048b8c:	7e e8                	jle    8048b76 <phase_2+0x2e>
 8048b8e:	8d 65 d8             	lea    esp,[ebp-0x28]
 8048b91:	5b                   	pop    ebx
 8048b92:	5e                   	pop    esi
 8048b93:	89 ec                	mov    esp,ebp
 8048b95:	5d                   	pop    ebp
 8048b96:	c3                   	ret
 8048b97:	90                   	nop
```

```c
read_six_numbers() {

}

phase_2()
{
	int num = read_six_numbers();
	//
}


```
