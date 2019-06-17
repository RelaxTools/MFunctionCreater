# MLangBuilder
Power Querry M Language Builder For VBA  
VBA �� Powr Query �� M���� ��g�ݗ��Ă�Ƃ��ɕ֗��ȃN���X�Q

## �N���X�ꗗ

| �N���X | ���� |
----|---- 
| MCsv | Csv �̊֐��̓���N���X�B |
| MFile | File �̊֐��̓���N���X�B |
| MTable | Table �̊֐��̓���N���X�B |
| MRecord | Record �̑g�ݗ��ăN���X(Dictionary���b�p�[�N���X)�B |
| MList | List �̑g�ݗ��ăN���X(Collection���b�p�[�N���X)�B |
| MCommand | �e�N���X����M��������o���N���X |

## �Ăяo���T���v��
```
'------------------------------------------------
' MCommand��VBA�ō쐬����ꍇ�̃w���p�[�N���X
'------------------------------------------------
Sub Sample()

    '-----------------------------------
    ' MCommand���������ɍ쐬����ꍇ
    '-----------------------------------
    Dim t1 As MTable
    Dim t2 As MTable
    Dim t3 As MTable
    
    Set t1 = MCsv.Document(MFile.Contents("C:\Test.csv"), "[Delimiter="","", Columns=5, Encoding=65001, QuoteStyle=QuoteStyle.Csv]")
    Set t2 = MTable.Skip(t1, 2)
    Set t3 = MTable.PromoteHeaders(t2, "[PromoteAllScalars=true]")

    Dim m1 As MCommand
    Set m1 = New MCommand
    
    m1.Append t3
    Debug.Print m1.ToString
    
    '����
    'let Source1 = Table.PromoteHeaders(Table.Skip(Csv.Document(File.Contents("C:\Test.csv"),
    '              [Delimiter=",", Columns=5, Encoding=65001, QuoteStyle=QuoteStyle.Csv]), 2), [PromoteAllScalars=true]) in Source1

    
    '-----------------------------------
    ' MCommand�ɑ�����č쐬����ꍇ
    '-----------------------------------
    Dim m2 As MCommand
    Set m2 = New MCommand
    
    m2.Append MCsv.Document(MFile.Contents("C:\Test.csv"), "[Delimiter="","", Columns=5, Encoding=65001, QuoteStyle=QuoteStyle.Csv]")
    m2.Append MTable.Skip(m2.Table, 2)
    m2.Append MTable.PromoteHeaders(m2.Table, "[PromoteAllScalars=true]")

    Debug.Print m2.ToString

    '����
    'let Source1 = Csv.Document(File.Contents("C:\Test.csv"), [Delimiter=",", Columns=5, Encoding=65001, QuoteStyle=QuoteStyle.Csv]),
    '    Source2 = Table.Skip(Source1, 2),
    '    Source3 = Table.PromoteHeaders(Source2, [PromoteAllScalars=true]) in Source3


    '-----------------------------------
    ' MRecord/MList��p�����T���v��
    '-----------------------------------
    Dim m3 As MCommand
    
    'MRecord(M�����Record) �� Dictionary��Wrap�������́B�g�p���@��Dictionary�����B
    Dim rec As MRecord
    Set rec = New MRecord
            
    rec.Add "Column1", """No."""
    rec.Add "Column2", """NAME"""
    rec.Add "Column3", """AGE"""
    rec.Add "Column4", """ADDRESS"""
    rec.Add "Column5", """TEL"""

    'MList(M�����List) �� Collection��Wrap�������́B�g�p���@��Collection�Ɠ����B
    Dim lst As MList
    Set lst = New MList
    lst.Add rec
    
    Set m3 = New MCommand

    m3.Append MCsv.Document(MFile.Contents("C:\Test.csv"), "[Delimiter="","", Columns=5, Encoding=65001, QuoteStyle=QuoteStyle.Csv]")
    m3.Append MTable.Skip(m3.Table, 2)
    m3.Append MTable.InsertRows(m3.Table, 0, lst)
    m3.Append MTable.PromoteHeaders(m3.Table, "[PromoteAllScalars=true]")

    Debug.Print m3.ToString

    '����
    'let Source1 = Csv.Document(File.Contents("C:\Test.csv"), [Delimiter=",", Columns=5, Encoding=65001, QuoteStyle=QuoteStyle.Csv]),
    '    Source2 = Table.Skip(Source1, 2),
    '    Source3 = Table.InsertRows(Source2, 0, {[Column1="No.", Column2="NAME", Column3="AGE", Column4="ADDRESS", Column5="TEL"]}),
    '    Source4 = Table.PromoteHeaders(Source3, [PromoteAllScalars=true]) in Source4

End Sub

```
