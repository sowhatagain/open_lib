# ����������� ���� ����� �� �������, ��������.

���������� ������ �������� ���������-���������� www.openlibrary.org


# 1) ������ (����� Data)


https://openlibrary.org/data - ���������� �� ������ � ������� �����


��������� �� �������� �����, ���������� � ����� ����� ����� ������ � 2000 �� 2018 ��


Fantasy 5670 
Medicine 4383 
Music 9771 
Mystery and detective stories 2534 
Religion 14901 
Romance 11284 
Recipes 1531 
Science 16848 
Children 5766
Science fiction 4677


��� ��������� json c ������� � �������� ������������ ������ ����: http://openlibrary.org/subjects/key.json?published_in=2000-2018&limit=books_number (����� ���� �� ���� � ����� .json)
��� � key ���������� ��������, � ����� ����� ������������� ����.


��� ���������� ����������� - c����� ���� subject.ipynb. ������ ������ � ����� ����� �������� ������� ���� ��������� ����. ����������� ����������� ���������, ����� �������� ������� ��������� ���� ����� ����������.


# 2) EDA (����� Preprocessing)


������ ������ � ����������� ����, �������������.


�) ��������� ���� - ������������ � data_preprocessing.ipynb ������� ��. � �����.


    * �������� ��������� �� ��������
    * �� ����� � ������ ��������� � ������� �� ���������� �������� (�������� � ����. ������)
    * � ������� ��������� ����� �����������, ����������� ������� �������� ��������� �������������, � word2vec �������� ��������� � ������� ���� ���������� �����
    * ����� ���������� ���������, ������� ������ ���������� 2-3 �������, ������� ������ ������������ ������������� �� ������� ������� word2vec
    * wordcloud ������� ��������������, ��� �������������, � ���� ����� �������� ���� ������������� � ��������� �� ������ �����

�������� ����������� - ��������������� ������ �������������. ���� � �� �� ����� ����������� ���������� �������. ����� ������������� ������������� ������� �� �����. �� ���� ��������� ����� �� ��������� ����� ������� ������ � downsampling-y, ����� �������� ������ ���� �� ���� ���������� ��� ������������� ���� one-to-all. �������� ������� upsampling "������� �� ������ ����������" �� �������������� ���������.


�) ����������� - ��������� ����������� ������� �������� �� �������� � ������������� � ����� �� ��������, ������� ������ ���������� � ��������� ( 224�224�3 px), ����� �� ������� �� ��� ����� ������ ������. img_changes_size_name.ipynb ����������, ��������� ����� ����� (����� ��� ��������������� �������������)


# 3) ������������ �� ����������:

������������ ������ � �������������, ��� ����� ������������� ����� one-to-all ��� 10-�� �������, ������� �� ������ ���� ��� 10 ������ �������. ����� �������, "�������" �������� ����� 10 ������������� �������, ������� �� ������ �������������� ������ ��� ���������� �������.


train/test ��������� � �������� 80/20 (��-�� ��������� ������ train ��������� �� 80)


��� ����, ����� �������� ������ ���������, �� ������� ����� ������� ��������� ���� ��� ����� ����� � ��������� ����� ���������� �� ������������� ������ word2vec �� Google.


� ���������� ������������� ������������ �� ���������������� ��������� ���� �� ������ LSTM, ������� ���� ����������� "����������" ���������� ��������� � �����, ��� ����� ��� ������.


35 ���� �������� ����������, ����� ��������� ������ ������� ���������� ����������, ������ ������� ��������� ���������� � 20-25 ������. ������������� ������ �� �������� ����� 3 � ��������� ����. ����������: ��������� ���� � LSTM. ���� ������� ���������� ������������� �� �������� ������ ������� precision, recall � f1-���� ������ ��� ������ ������� ����������� � �������� 0,7-0,85.


������� ������ ��� ������ ��� ������ ������������������� �� ������� ���� ��-�� �������������� �� �������.


# 4) ������������ �� ������������ (����� 4.Cover)


��� ����������� ����� � ������� ������������ �� cover-id, ��� ��������� ������ �� ��������� ������. (����������, ��������, ������� ����������� �������� � �������� ��� ������)


�) ���������� one-to-all, ����������� �� ��, ��� � ��� ����������. ������: cover-lstm.ipynb


2 ����� train/test � ��������� 80/20 � ����� ����������� ������ � ���������� "������" �� ������ ����� CNN ����� ���������� ������ �������� ���� ���� ��������� ������������: �� ���� ������������� ���� ResNet. ���� ����������. ����� ���� ���������� �� ���� ��� ��������� LSTM-����� � 1-�� �������. Precision, recall � f1-���� ��� ����� ������� �������� ������ 0,65-0,75


�) ��������������� ���������� ������� ������ �� ������������� ����������: GPU Nvidia 1080, ���������� ����. ���������� 8 ��.


������: cover-multiclass.ipynb �� ���� ������������� ���� ResNet. ���� ����������. ����� ���� ���������� �� ���� ��� ��������� dense-����� � 10-� �������� �� ���-�� �������. train/test ����������� � ��������� 80/20. Train_test_split �������� ������� ��������������� ���� �������.


������������ �� ������: ����� ������������� ���� ResNet ��� ��������� ������������� �������� �����, ��������������� � ��, �� ������ ������ ���� "����������" � ��������� ��������. Accuracy �������� � ������������� ������� "����������" � �������� 0,25 +- � �� ������� �� ���������� ����� ���������� ��������. � ����� �������� ������ ������������� ������������ ��� ����������� �������������� ������.


ResNet c ������ ������������ ������, �������� ������������ �������� �������.


�����, ��� �������, ���� ��, ��� � ���� �������� ����������������� �� ������� ������. � ���� ������ ��������� �� ������� downsampling �� ��������� � �������� ���������������? (������) �������� �� ��, ��� ������ ��������������� (���� �� ������). ���� ���� ������� � ���������� �����.




# 5) ������ �� ����������� �������:

�� ��������� ����� ���������� ���������� ������ ������������� ���� �����. ��� ����������� ������� �������� ����� CNN+LSTM ������ �����, ����������� ��������� ������, ������������ � ������.


# 6) ��������� �� ����� ������. (����� 5.Validation/cover)

�)  
���� - cover-lstm-product.ipynb

� �������� �������� ������� ����������� ���� ��� ������� � ���������� loss. �������� ���� ��/ � ������, ������������ ���� �� �������. ��������� ������ ���������� ����� � ����� '�������'. ��������� ������ �������, recall �������� 0,83 �� ����� ����� ����. ����� �������� classification report.
 Precision ��� �������� ������ ���, ������, �������� ��� ���������� ������. ��������� ������: � ��� ����? ��� ������: ������� ����������� ����� �������: ������ ���, ����� �� ������ �������� 50 ����, ���� �� ������� �� ��� ����������� � ���������� �� ���� ��� (1998). ������� ����� ���� ������� ������������ ����� �� ���� ������� ������ � ��������� �����������. ��� �������� ���� - ��� 0,08 �� ���� ����. ��� ����� ������ � precision � ���������� ������� ������ ��������� ������
 
�) ��������� �� �������� � �������� ������, �� ����� ��������� ������� ���������.


# 7) ���������� ���������:

����� - sklearn �������� ���������� ��������, ������� ����� �������� � ���������������� ��������. ���������� � ���� ��������������� � ������� �� ����������� � ������������� ���� ������. (http://scikit-learn.org/stable/modules/multiclass.html) ����� ���������� � �������� �� ��������, �� ��� �� ������� �� �����������������.

���������:

    ���� ����������� ���������� � ������-����������� �� API (������), ��������� ��������� ����� �������� � �������� �� � ��������.
    ��� ���� ����������� �������� ����� ������ (���������), ��������� ������������ word2vec, ��� ���� ������ ������� ���������� �������������.

�����������:

    ��������� ����� �������� ������������ � ���������� �� ���������, ����� ���� � ������ ������������ ������ ��� ������� � �������������.
    image agumentation ����� ����������� ����� ����� ������ �������� �������� �������������.
    �������� ������� ������: ���������� � �������� ����� �� ������ ���������������� ������������� �� imagenet �������, ���������� ����������� � ����� �������� ������ �� ������ ���� ����� ������ ����������� ������������ ������ �� ���������. ������� ����� ����� ����� ��������� � �������� �������� ��� �� ������ ��������.

����� ������� � ������� ��������������� ������, ����� �� ��������� �� �� ��������� ��������.

� ������, ���������� �������-������� - ��� ��������, ������� ����� ���������� ����� ������� � ������ ��������� ���������.