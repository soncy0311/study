# Voting Ensemble (Classifier)
: 여러 모델을 ensemble 하는 과정을 모델간의 투표(voting)으로 진행

## Hard Voting
: a,b,c,d의 모델로 0,1 이진분류 모형의 ensemble을 예로 들자.<br>
 예측 결과가 {a:1, b:0, c:1, d:1}일 경우 1을 선택한 경우가 더 많아 1을 선택한다는 내용<br>
 이 경우 정확한 모델의 결과를 무시하고 진행될 수 있어서 조심할 필요가 있어보인다.

## Soft Voting
: 예측 결과 확률값을 모두 더한 후 그 값중에 큰 값을 선택하는 형태로 투표를 한다.
 위와 같은 예시에서 예측결과가 [{a: {0:0.4, 1:0.6}}, {b: {0:0.4, 1:0.6}}, {c: {0:0.9, 1:0.1}}, {d: {0:0.4, 1:0.6}}]일 경우 {0: 2.1, 1:1.9}로 선택결과는 0=1, 1=3 이지만 모델은 0을 채택한다.

#### 사용방법
1. 같은 모델에 대해서 K-Fold 방법으로 train, valid 셋을 나누고  

### code
```python
from xgboost import XGBClassifier
from sklearn.linear_model import LogisticRegression
from sklearn.ensemble import RandomForestClassifier, AdaBoostClassifier, ExtraTreesClassifier, BaggingClassifier, GradientBoostingClassifier, VotingClassifier
from sklearn.svm import SVC
from sklearn.tree import DecisionTreeClassifier

clf1 = XGBClassifier()
clf2 = RandomForestClassifier()
clf3 = DecisionTreeClassifier()
clf4 = LogisticRegression()
clf5 = AdaBoostClassifier()
clf6 = ExtraTreesClassifier()
clf7 = BaggingClassifier()
clf8 = GradientBoostingClassifier()
clf9 = SVC(probability=True)

classifier = [('xgb', clf1),('rf',clf2),('dt',clf3),('logit',clf4),('ada',clf5),
             ('ext',clf6),('bagg',clf7),('gdb',clf8),('svc',clf9)]

voting = VotingClassifier(estimators=classifier,voting='soft')
voting.fit(X, y)
```