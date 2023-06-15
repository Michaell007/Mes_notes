# Comment enregistrer les données d'un fichier Excel vers un SGBD (ex: MySQL) ?

> On va le faire avec **jupyter dans le navigateur** sans Anaconda !!!

 - Install -  pip install jupyterlab
 - Launch jupyter - jupyter lab
 > S'assurer d'avoir aussi installer ces librairies
 
 - pip install pandas
 - pip install mysql-connector
 - pip install openpyxl

# Le scripts

    import pandas as pd
    import warnings
    import mysql.connector
    warnings.filterwarnings('ignore')

> Ouverture du fichier Excel en sélectionnant le nom de la feuille

    df = pd.read_excel(r"TB_PERSON.xlsx", sheet_name="tb_person")
    df.head()

> Mettre tous les champs vide en chaine de caractères

    df.fillna('', inplace=True) 

> Connection a la base de données

    try:
        cnx = mysql.connector.connect(user=username, password=password, host=server)
        cursor = cnx.cursor()
        print('connected')
    except Exception as e:
        print(f"Erreur : {e}")

> Itération sur les éléments du fichier

    for i in df.iterrows():
        print(i[1])
    cursor.close()

Pour plus de fonctionnalités, il faudra consulter la doc de la librairie pandas

> Exemple complet du code

    import pandas as pd
    import warnings
    import mysql.connector
    warnings.filterwarnings('ignore')
    
    df = pd.read_excel(r"Extraction-users-departements.xlsx", sheet_name="DEPARTEMENTS")
    df.head()
    
    df.fillna('', inplace=True)
    
    try:
        cnx = mysql.connector.connect(user='root', password='', host='localhost')
        cursor = cnx.cursor()
        print('connected')
    except Exception as e:
        print(f"Erreur : {e}")
        
    try:
        for i in df.iterrows():
            # print( type( i[1][10] ) )
            cursor.execute("INSERT INTO carte_digitale.departements (CODEDEPARTEMENT_POLITIK, NOMDEPARTEMENT_POLITIQUE, CODEDEPARTEMENT_ADMIN, ID_AUTO) VALUES (%s, %s,  %s, %s)", (i[1][0],i[1][1],i[1][2],i[1][3]))
            cnx.commit()
        cursor.close()
    except Exception as e:
        print(f"Erreur : {e} {e.args}")
