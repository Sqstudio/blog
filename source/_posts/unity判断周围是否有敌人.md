---
title: Unity判断周围是否有敌人
tags: []
id: '1368'
categories:
  - - Unity3D
date: 2017-09-01 20:41:06
---

只攻击正前方的单位，向前发射一条射线，攻击碰到的单位

RaycastHit hit;
//range 射线的长度，即攻击范围，maskTarget敌方单位的mask，只攻击敌方单位
if(Physics.Raycast(unit.thisT.position, unit.thisT.forward, out hit, range, maskTarget)){
      Unit targetTemp=hit.collider.gameObject.GetComponent();
      if(targetTemp!=null && targetTemp.HPAttribute.HP>0){
           target=targetTemp;
           if(attackMode==\_AttackMode.StopNAttack){
               if(attackMethod!=\_AttackMethod.Melee) unit.StopAnimation();
                   unit.StopMoving();
               }
      }
}

以己方单位为圆心的某一半径长度内

//返回相交球的所有碰撞体
Collider\[\] cols=Physics.OverlapSphere(unit.thisT.position, range, maskTarget);
//if(cols!=null && cols.Length>0) Debug.Log(cols\[0\]);
if(cols.Length>0){
       Collider currentCollider=cols\[Random.Range(0, cols.Length)\];
       Unit targetTemp=currentCollider.gameObject.GetComponent();
       if(targetTemp!=null && targetTemp.HPAttribute.HP>0){
             target=targetTemp;
             if(attackMode==\_AttackMode.StopNAttack){
                if(attackMethod!=\_AttackMethod.Melee) unit.StopAnimation();
                   unit.StopMoving();
             }
       }
}

以己方单位为圆心的扇形范围内

Collider\[\] cols=Physics.OverlapSphere(unit.thisT.position, range, maskTarget);
//if(cols!=null && cols.Length>0) Debug.Log(cols\[0\]);
if(cols.Length>0){
      Collider currentCollider=cols\[0\];
      foreach(Collider col in cols){
          Quaternion targetRot=Quaternion.LookRotation(col.transform.position-unit.thisT.position);
          if(Quaternion.Angle(targetRot, unit.thisT.rotation)
             Unit targetTemp=currentCollider.gameObject.GetComponent();
             if(targetTemp!=null && targetTemp.HPAttribute.HP>0){
                  target=targetTemp;
                  if(attackMode==\_AttackMode.StopNAttack){
                       if(attackMethod!=\_AttackMethod.Melee) unit.StopAnimation();
                        unit.StopMoving();
                  }
                  break;
              }
          }
     }
}